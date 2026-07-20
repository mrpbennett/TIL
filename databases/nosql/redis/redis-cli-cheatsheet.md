# Redis CLI Cheat Sheet

A working reference for `redis-cli` — connecting, reading data back out, and the
admin/dev commands worth having memorised when you're poking around a Redis
instance.

## Connecting

```bash
redis-cli                          # connect to localhost:6379
redis-cli -h <host> -p <port>      # connect to a remote instance
redis-cli -h <host> -p <port> -a <password>   # with auth (prefer --user/--pass env in scripts)
redis-cli -n 2                     # connect and SELECT db 2
redis-cli --tls                    # connect over TLS
```

Once connected, `PING` should return `PONG` — the fastest way to confirm the
server is alive and reachable.

## Reading data

These are the commands you reach for once you already know (or are figuring
out) what's stored.

```bash
TYPE user:100                # what kind of value is this key? string/hash/list/set/zset/stream
GET user:100                 # read a string value
MGET user:100 user:101       # read multiple string values in one round trip
STRLEN user:100              # length of a string value

HGETALL user:100             # all fields + values of a hash
HGET user:100 email          # a single hash field
HKEYS user:100                # just the field names
HLEN user:100                 # number of fields

LRANGE mylist 0 -1           # every element of a list (0 to -1 = full range)
LLEN mylist                  # length of a list

SMEMBERS myset                # every member of a set
SISMEMBER myset foo           # is "foo" a member?
SCARD myset                   # set size

ZRANGE myzset 0 -1 WITHSCORES  # sorted set members + scores, ascending
ZREVRANGE myzset 0 -1          # sorted set, descending
ZSCORE myzset foo              # score of one member

XRANGE mystream - +           # every entry in a stream
```

### Finding keys

```bash
KEYS pattern*        # blocks the server while it scans ALL keys — dev/small datasets only, never production
SCAN 0 MATCH pattern* COUNT 100   # cursor-based, non-blocking iteration — use this instead of KEYS
redis-cli --scan --pattern 'user:*'   # same idea, but redis-cli drives the cursor loop for you
```

`SCAN` returns a cursor; keep passing that cursor back in until it comes back
as `0` to know you've iterated the full keyspace.

### Expiry

```bash
TTL user:100          # seconds until expiry, -1 = no expiry, -2 = key doesn't exist
PTTL user:100          # same, in milliseconds
EXPIRE user:100 3600   # set a 1 hour TTL
PERSIST user:100       # remove the TTL, key lives forever again
```

## Admin / dev commands

The ones worth knowing beyond basic reads — inspecting the server, debugging
live traffic, and understanding what's actually in memory.

```bash
INFO                        # full server stats: memory, clients, replication, persistence, keyspace
INFO memory                 # just the memory section
DBSIZE                      # number of keys in the current logical database
SELECT 1                    # switch logical database (0-15 by default)

CONFIG GET maxmemory        # read a config value
CONFIG SET maxmemory 100mb  # change a config value at runtime (not persisted to redis.conf)
CONFIG GET maxmemory-policy # check the eviction policy

CLIENT LIST                 # every connected client, with idle time, address, last command
CLIENT INFO                 # details of the current connection

MONITOR                     # stream every command hitting the server in real time — great for debugging, never leave running on a busy prod instance (huge perf hit)

MEMORY USAGE user:100       # bytes a specific key is using
MEMORY DOCTOR               # Redis's own opinion on your memory situation

SLOWLOG GET 10              # last 10 commands that exceeded the slow-log threshold
SLOWLOG RESET                # clear the slow log

BGSAVE                       # trigger an async RDB snapshot to disk
BGREWRITEAOF                 # trigger an async AOF rewrite/compaction
LASTSAVE                     # unix timestamp of the last successful BGSAVE

FLUSHDB                      # wipe every key in the current database
FLUSHALL                     # wipe every key in every database — both are irreversible, guard with confirmation in scripts
```

## Notes

- Prefer `SCAN` over `KEYS` and avoid leaving `MONITOR` running — both can
  block or slow down a production instance.
- `CONFIG SET` changes are in-memory only; add them to `redis.conf` (or your
  config management) if they need to survive a restart.
- `FLUSHDB`/`FLUSHALL` have no confirmation prompt from the CLI — think twice
  before running them anywhere near production.

[Redis command reference](https://redis.io/commands/)
