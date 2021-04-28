# Connecting to Presto DB

How to connect to PrestoDB via Python using [presto-python-client](https://github.com/prestodb/presto-python-client)

The `BasicAuthentication` class can be used to connect to a LDAP-configured Presto cluster:

```python
import prestodb

try:
        conn = prestodb.dbapi.connect(
            host="host url",
            port=1234,
            user="username",
            catalog="catalog",
            schema="schema",
            http_scheme="https",
            auth=prestodb.auth.BasicAuthentication("principal id", "password"),
        )
        cur = conn.cursor()
        cur.execute("SELECT * FROM some.database"
        )
        rows = cur.fetchall()
    except prestodb.exceptions.HttpError as err:
        raise SystemExit(err)
```

Everything above is quite self explainitory however I wasnt to sure what `principal id` was. After asking the right people at my place of work, it was the same as my `username`.
