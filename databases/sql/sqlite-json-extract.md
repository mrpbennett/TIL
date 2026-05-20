# Extracting JSON Fields as Columns in SQLite

SQLite ships with first-class JSON support — if you've stashed a JSON blob in a
`TEXT` column (e.g. a webhook payload), you don't need to parse it in your
application code. You can pull fields out as real columns at query time using
`json_extract()` or the shorthand `->>` operator.

Given a column `payload` containing something like:

```json
{
  "dataLayer": [{"gtm.start": 1779278496121, "event": "gtm.js"}],
  "connection": "4g",
  "vendor": "Google Inc."
}
```

You can flatten it into columns like this:

```sql
SELECT
  json_extract(payload, '$.connection')          AS connection,
  json_extract(payload, '$.vendor')              AS vendor,
  json_extract(payload, '$.dataLayer[0].event')  AS first_event
FROM events;
```

Or, more concisely with the `->>` operator (SQLite 3.38+):

```sql
SELECT
  payload ->> '$.connection'         AS connection,
  payload ->> '$.vendor'             AS vendor,
  payload ->> '$.dataLayer[0].event' AS first_event
FROM events;
```

## `->` vs `->>`

- `->` returns JSON (use when you want to keep drilling deeper).
- `->>` returns a SQL scalar — text, number, etc. This is almost always what
  you want for output columns.

## Path syntax gotchas

- **Array indexing**: `$.dataLayer[0].event`.
- **Keys containing dots** (`gtm.start`, `gtm.uniqueEventId`) need quoting:
  `$."gtm.start"` or `$.dataLayer[0]."gtm.start"`. Without the quotes SQLite
  treats the dot as a path separator and silently returns `NULL`.

## Exploding arrays into rows

`json_each()` turns a JSON array into a virtual table — one row per element —
which you can join against the source table:

```sql
SELECT
  e.id,
  je.value ->> '$.event'               AS event,
  je.value ->> '$."gtm.uniqueEventId"' AS uid
FROM events e, json_each(e.payload, '$.dataLayer') je;
```

## Filtering and indexing

JSON paths work in `WHERE` clauses too:

```sql
SELECT * FROM events WHERE payload ->> '$.vendor' = 'Google Inc.';
```

If a particular field gets queried often, an **expression index** keeps it
fast without denormalising the schema:

```sql
CREATE INDEX idx_events_vendor
  ON events (json_extract(payload, '$.vendor'));
```

This pattern is great for ingestion-heavy workloads (webhooks, event logs)
where the source shape is variable but you want SQL-shaped reads on the way
out.
