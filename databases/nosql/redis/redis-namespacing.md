# Redis Namespacing

Redis has no built-in namespace system — it's a flat keyspace. Namespacing is
a set of conventions and mechanisms layered on top.

## Key Naming Conventions (Primary Approach)

The most universal strategy is a **colon-delimited prefix** on every key:

```
<app>:<domain>:<identifier>
```

Examples:

```
billing:product:1042
auth:session:abc123
cache:billing:product:1042
prod:billing:invoice:9910
```

Redis keys are arbitrary binary-safe strings — colons carry no special meaning
to Redis itself. The convention exists for human readability and tooling
support. Many Redis GUIs (RedisInsight, Medis) auto-group keys by colon
segments into a tree view.

## Logical Databases (`SELECT`)

Redis ships with 16 logical databases (0–15) by default, configurable via
`databases` in `redis.conf`.

```redis
SELECT 1       # switch to DB 1
SET foo bar    # key lives only in DB 1
SELECT 0       # back to default
GET foo        # returns nil — different namespace
```

Keys in different DBs can share the same name without collision. `FLUSHDB`
only flushes the currently selected database.

**Critical limitations:**

- Not supported in Redis Cluster
- Pub/Sub is global across all DBs
- `MOVE key db` is the only cross-DB operation — no atomic multi-DB transactions
- Same process, same memory, same RDB/AOF file — not true isolation

Redis's own guidance: use logical DBs to separate keys *within* the same
application (e.g. dev vs. test locally), not to isolate unrelated applications.

## Querying Namespaced Keys

**`KEYS pattern`** — never in production:

```redis
KEYS billing:*          # returns all billing keys
KEYS billing:product:*  # narrows to products
```

`KEYS` blocks the entire Redis event loop while scanning. On a DB with millions
of keys this can pause Redis for seconds.

**`SCAN`** — the production-safe alternative:

```redis
SCAN 0 MATCH "billing:product:*" COUNT 100
```

`SCAN` is cursor-based and non-blocking. Loop until the cursor returns `0`:

```python
cursor = 0
keys = []
while True:
    cursor, batch = r.scan(cursor, match="billing:product:*", count=100)
    keys.extend(batch)
    if cursor == 0:
        break
```

`COUNT` is a hint, not a guarantee — Redis may return more or fewer results.

## ACL-Based Namespacing (Redis 6+)

ACLs enforce namespace isolation at the server level, not just by convention:

```redis
ACL SETUSER billing_service on >password ~billing:* +@all
ACL SETUSER auth_service on >password ~auth:* +@all
```

Attempting to `GET auth:session:123` from `billing_service` returns a `NOPERM`
error.

Redis 7.2+ adds granular key permissions:

```
%R~billing:*     # read-only on billing keys
%W~billing:*     # write-only on billing keys
%RW~billing:*    # full access on billing keys
```

This is the strongest form of namespacing Redis offers — enforcement moves from
application code into the server itself.

## Multi-Tenant Namespacing Patterns

When multiple apps or tenants share one Redis instance:

```
# Application-level
cache:billing:product:42
cache:auth:token:xyz

# Tenant-scoped
tenant_9910:settings:theme
tenant_9910:session:abc

# Environment prefix
prod:cache:user:1
staging:cache:user:1
```

Tenant-scoped prefixes let you `SCAN tenant_9910:*` to enumerate or purge an
entire tenant's data in one sweep.

## Redis Cluster: Hash Tags

Redis Cluster shards data across nodes using 16,384 hash slots. By default the
hash is computed from the full key name. **Hash tags** force co-location:

```
{billing}:product:42
{billing}:invoice:99
```

Keys with the same `{hash tag}` always land on the same slot/node. This is
required for multi-key commands (`MGET`, `MSET`, transactions, Lua scripts),
which fail if keys span multiple nodes.

`SELECT` / logical databases are unavailable in Cluster mode.

## Which Mechanism to Use

| Need | Mechanism |
|---|---|
| Organize keys by app/domain/resource | Colon-delimited key prefixes |
| Separate environments locally | Logical databases (`SELECT`) |
| Enforce isolation in production | ACLs with key patterns |
| Enumerate a namespace safely | `SCAN` with `MATCH` |
| Co-locate keys on the same cluster node | Hash tags `{tag}` |
| True isolation (different teams/security domains) | Separate Redis instances |

The practical default for most production systems: **key prefixes + ACLs**.
Logical databases are a footgun at scale — avoid them outside of local dev.
