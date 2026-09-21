# PostgreSQL Indexing: Why, and Which Type

An index lets Postgres avoid a full sequential scan when answering a query.
Without one, a lookup like this has to walk every row in `test1`:

```sql
CREATE TABLE test1 (
    id integer,
    content varchar
);

SELECT content FROM test1 WHERE id = constant;
```

Add an index on the column that's searched, and the planner can walk a
search tree a few levels deep instead of scanning the whole table:

```sql
CREATE INDEX test1_id_index ON test1 (id);
```

Pick a name you'll recognise later — Postgres doesn't care what it's
called. `DROP INDEX test1_id_index;` removes it again; indexes can be
added and dropped at any time without touching the data.

## What it costs

- Once created, the index maintains itself automatically on
  `INSERT`/`UPDATE`/`DELETE` — but that maintenance is overhead on every
  write, so indexes that are never used by a query should be dropped.
- `ANALYZE` needs to run (usually via autovacuum) so the planner's
  statistics stay accurate enough to decide when using the index is
  actually cheaper than a sequential scan.
- Building an index on a large table blocks writes until it finishes,
  unless you use `CREATE INDEX CONCURRENTLY`, which allows writes to
  continue in exchange for a slower build and some extra caveats.
- Indexes also help `UPDATE`/`DELETE` search conditions and join
  columns — not just `SELECT`.

## The index types

`CREATE INDEX` defaults to a B-tree. Other types are chosen with `USING`:

```sql
CREATE INDEX name ON table USING HASH (column);
```

| Type      | Best for                                                                 |
| --------- | ------------------------------------------------------------------------ |
| **B-tree**   | Default. Equality and range comparisons (`<`, `<=`, `=`, `>=`, `>`), `BETWEEN`, `IN`, `IS NULL`, prefix `LIKE 'foo%'` / `~ '^foo'`, and returning rows in sorted order. |
| **Hash**     | Simple equality (`=`) only — stores a 32-bit hash of the column value. |
| **GiST**     | Not one algorithm but an extensible framework; built-in geometric operator classes support containment/overlap operators and "nearest-neighbor" ordering (e.g. `ORDER BY location <-> point '(x,y)'`). |
| **SP-GiST**  | Same idea as GiST but for non-balanced, disk-based structures — quadtrees, k-d trees, tries. Also supports nearest-neighbor searches for operator classes that define a distance ordering. |
| **GIN**      | "Inverted index" for values with multiple components, like arrays — one entry per component, so it efficiently answers "does this row contain X" queries (`@>`, `<@`, `&&`). |
| **BRIN**     | Block Range INdex — stores per-block-range summaries (e.g. min/max) instead of per-row data. Cheap and small, but only effective when column values correlate with physical row order. |

Choosing the wrong type still works — Postgres just won't use the index
efficiently (or at all) for a given query. B-tree covers the vast
majority of everyday equality/range lookups; reach for the others only
when the data shape calls for it (arrays → GIN, geometric/nearest-neighbor
→ GiST, huge append-only tables ordered by insertion → BRIN).

Sources:

- [11.1. Introduction — PostgreSQL Docs](https://www.postgresql.org/docs/current/indexes-intro.html)
- [11.2. Index Types — PostgreSQL Docs](https://www.postgresql.org/docs/current/indexes-types.html)
