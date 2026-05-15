# Using Postgres as a Key-Value Store

Postgres makes a competent Redis replacement when you already run it and
operational simplicity matters more than the last 10× of performance. It
won't beat Redis on raw latency, but with the right schema it handles tens
of thousands of ops/sec comfortably — one fewer moving part in the stack.

## The basic schema

```sql
CREATE UNLOGGED TABLE kv_store (
    key         TEXT PRIMARY KEY,
    value       JSONB NOT NULL,
    expires_at  TIMESTAMPTZ,
    updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_kv_expires ON kv_store (expires_at)
    WHERE expires_at IS NOT NULL;
```

Key design choices:

- **`UNLOGGED`** — skips the WAL for ~2–3× faster writes. Tradeoff: data is
  wiped on crash, so it's perfect for cache-like use and dangerous for
  source-of-truth data.
- **`JSONB`** — lets you store strings, numbers, objects, and arrays in a
  single column. Use `BYTEA` for raw binary or `TEXT` for pure strings.
- **Partial index on `expires_at`** — only indexes rows that actually have
  a TTL, keeping it small.

## Core operations

```sql
-- SET (upsert with TTL)
INSERT INTO kv_store (key, value, expires_at)
VALUES ($1, $2::jsonb, NOW() + INTERVAL '1 hour')
ON CONFLICT (key) DO UPDATE
    SET value = EXCLUDED.value,
        expires_at = EXCLUDED.expires_at,
        updated_at = NOW();

-- GET (TTL-aware)
SELECT value
FROM kv_store
WHERE key = $1
  AND (expires_at IS NULL OR expires_at > NOW());

-- INCR (atomic counter)
INSERT INTO kv_store (key, value)
VALUES ($1, '1'::jsonb)
ON CONFLICT (key) DO UPDATE
    SET value = to_jsonb((kv_store.value::int + 1))
RETURNING value;

-- MGET
SELECT key, value
FROM kv_store
WHERE key = ANY($1::text[])
  AND (expires_at IS NULL OR expires_at > NOW());
```

## Expiration

Postgres has no background expirer, so you have two options. **Lazy expiry**
is free — every read filters `expires_at > NOW()` and expired rows just sit
there. For active cleanup, schedule a `pg_cron` job:

```sql
SELECT cron.schedule('kv-cleanup', '*/5 * * * *', $$
    DELETE FROM kv_store
    WHERE expires_at IS NOT NULL AND expires_at < NOW()
$$);
```

## Pub/Sub via LISTEN/NOTIFY

Postgres has native pub/sub. Channels are ephemeral, payloads are capped at
8000 bytes — for larger messages, send a row ID and let the subscriber fetch.

```sql
-- subscriber
LISTEN cache_invalidations;

-- publisher
NOTIFY cache_invalidations, 'user:42';
```

## Redis data type equivalents

| Redis | Postgres approach |
|---|---|
| String | `TEXT` or `JSONB` value |
| Hash | `JSONB` object; mutate with `jsonb_set()` |
| List | `JSONB` array, or a table with a `position` column |
| Set | `JSONB` array, or a normalised join table |
| Sorted set | Table with `(key, member, score)` + index on `(key, score)` |
| Stream | Append-only table + `LISTEN/NOTIFY` for tailing |

## Performance notes

- **Connection pooling is mandatory.** Use PgBouncer in transaction mode —
  Postgres connections are ~10MB each, where Redis handles thousands cheaply.
- **Prepared statements** for hot paths to skip planning overhead.
- **`synchronous_commit = off`** for cache workloads — big write throughput
  win, tiny durability tradeoff.
- **Check `EXPLAIN ANALYZE`** on hot queries; the PK lookup should be
  `Index Scan using kv_store_pkey`, never a sequential scan.

## When NOT to do this

Use Redis (or KeyDB / Dragonfly) instead if you need sub-millisecond p99
latency, 100k+ ops/sec on a single node, Redis-specific structures
(HyperLogLog, Bitmaps, Geo, Streams consumer groups), or automatic eviction
under memory pressure — Postgres won't evict on its own.
