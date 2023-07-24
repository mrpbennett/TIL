# Redis Keys

Redis key names are unique by definition and are binary safe in Redis. Binary
safe being an example such as

```bash
"Foo", 42, 3.1415, 0xff
```

Keys names can also be up to 512MB in size.

[Generic Key commands](https://redis.io/commands#generic)

### Using the CLI

- **`SET`**: Set provides a way to sore a value for a given key name.

```
set user:100 paul
```

- **`GET`**: GET allows a user to getan individual key

```
get user:100
```

### `KEYS` / `SCAN`

There are two commands to gather a list of keys names, these are `KEYS` and
`SCAN`. They allow you to iterate over all keys in a Redis database or to get
all key matching a certain pattern.

The `KEYS` command always blocks entries iterated over all keys, if youre
database contains millions of keys it could block the database for a long time.
Therefore do not use in production.

On the other hand `SCAN` also blocks but only iterates over a handful of keys at
a time. Which then returns a slot reference where you're able to continue your
iteration.

## Key Expiration

| Set Expiration                                   | Inspect Expiration                     | Remove Expiration                            |
| ------------------------------------------------ | -------------------------------------- | -------------------------------------------- |
| [EXPIRE](https://redis.io/commands/expire)       | [PTTL](https://redis.io/commands/pttl) | [PERSIST](https://redis.io/commands/persist) |
| [EXPIREAT](https://redis.io/commands/expireat)   | [TTL](https://redis.io/commands/ttl)   |                                              |
| [PEXPIRE](https://redis.io/commands/pexpire)     |                                        |
| [PEXPIREAT](https://redis.io/commands/pexpireat) |                                        |

Keys also have a very useful property, you can define an expiration time or time
to live. Redis will keep the key in memory until space is required or is forced
out. Expiration times can be set in milliseconds, seconds or a UNIX timestamp.

Names are unique within the logical database. They are binary strings that can
represent any key name. They occupy a flat name space so you'll need to define a
consistent naming convention in order to separate concerns and domain objects.

Keys can be arbitrary large up to 512 megabytes although this is not really
recommended. The presence and absence of keys can be used to determine if a
value is set or not. And finally Redis can manage the automatic expiration of
keys through a TTL.

# Videos

- [KEYS](https://youtu.be/pcj9Q8dxvtU)
- [Expiration of Keys](https://youtu.be/a9m1Abg_Oyk)
