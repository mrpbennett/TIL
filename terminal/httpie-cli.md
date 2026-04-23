# HTTPie CLI

HTTPie (`http` / `https`) is a human-friendly command-line HTTP client that
makes API testing fast and readable. It handles JSON by default, colorises
output, and requires far less typing than curl.

## Basic syntax

```
http [FLAGS] [METHOD] URL [ITEM ...]
```

Method is optional — HTTPie infers GET when no data is given and POST when
data items are present.

## Request item separators

| Separator | Meaning                         | Example                        |
|-----------|----------------------------------|--------------------------------|
| `:`       | HTTP header                      | `X-Token:abc123`               |
| `==`      | Query parameter                  | `search==httpie`               |
| `=`       | JSON / form string field         | `name=Paul`                    |
| `:=`      | Raw JSON value (non-string)      | `age:=29 active:=true`         |
| `@`       | File upload / body from file     | `file@/path/to/data.json`      |

## JSON (the default)

When data fields are present, HTTPie automatically sets:
- `Content-Type: application/json`
- `Accept: application/json, */*;q=0.5`

```bash
# POST with JSON body
http POST api.example.com/users name=Paul age:=30 admin:=false

# Nested JSON via bracket notation
http POST api.example.com/app platform[name]=HTTPie platform[stars]:=54000
```

## Non-string values

Use `:=` for booleans, numbers, arrays, and objects:

```bash
http POST api.example.com/users \
  name=Paul \
  age:=29 \
  married:=false \
  hobbies:='["http", "pies"]'
```

## Sending raw JSON from a file

```bash
# Pipe stdin
cat payload.json | http POST api.example.com/endpoint

# Reference a file directly
http PUT api.example.com/endpoint @payload.json
```

## Output control

```bash
http -h GET api.example.com   # response headers only
http -b GET api.example.com   # response body only
http -v GET api.example.com   # full request + response
http --offline POST api.example.com name=test  # preview without sending
```

## Localhost shorthand

```bash
http :8080/health          # → http://localhost:8080/health
http :3000/api/users       # → http://localhost:3000/api/users
```

## Authentication

```bash
http -a user:pass api.example.com/secure      # Basic auth
http -A bearer -a $TOKEN api.example.com/me   # Bearer token
```

## Named sessions (persist cookies & auth)

```bash
http --session=myapp -a user:pass api.example.com/login
http --session=myapp api.example.com/profile   # auth reused
```

## Useful flags for scripting

```bash
--check-status      # exit non-zero on 4xx/5xx
--ignore-stdin      # prevent hanging in non-interactive scripts
--timeout=5         # connection timeout in seconds
--follow            # follow redirects
--verify=no         # skip SSL verification
--download          # wget-style file download with progress bar
```

Always pair `--ignore-stdin` with `--check-status` in CI/automation contexts
to avoid hangs and silent failures.

## Resources

- [Official docs](https://httpie.io/docs/cli)
