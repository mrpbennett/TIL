# Using SQLite as a Key-Value Store

SQLite ships with Python's standard library (`sqlite3`) and makes a solid
embedded KV store when you need persistence without the overhead of a
dedicated database server like Redis or a raw JSON file that grows unbounded.

The pattern is simple: a table with two TEXT columns — one for the key
(declared `PRIMARY KEY`) and one for the value. `INSERT OR REPLACE` gives you
upsert semantics so re-runs are idempotent.

```python
import sqlite3

con = sqlite3.connect("kv_data/my_store.db")
con.execute(
    "CREATE TABLE IF NOT EXISTS kv (key TEXT PRIMARY KEY, value TEXT NOT NULL)"
)

# Bulk load from a dict
data = {"abc123": "foo", "def456": "bar"}
con.executemany(
    "INSERT OR REPLACE INTO kv (key, value) VALUES (?, ?)",
    data.items(),
)
con.commit()

# Lookup
row = con.execute("SELECT value FROM kv WHERE key = ?", ("abc123",)).fetchone()
print(row[0])  # foo

con.close()
```

## Why this over a JSON file

| | JSON file | SQLite KV |
|---|---|---|
| Random lookup | O(n) parse + scan | O(log n) via B-tree index |
| File size | Grows linearly, all in memory | Paged on disk |
| Concurrent reads | Read locks the whole file | WAL mode allows readers + writer |
| Stdlib only | Yes | Yes |

For small datasets (< ~1 k entries) the difference is negligible. At tens of
thousands of entries SQLite lookups stay fast while the JSON parse cost starts
to show.

## Enabling WAL for concurrent access

```python
con = sqlite3.connect("kv_data/my_store.db")
con.execute("PRAGMA journal_mode=WAL")
```

WAL (Write-Ahead Logging) lets multiple readers proceed without blocking a
writer — worth enabling if the store is shared between processes.
