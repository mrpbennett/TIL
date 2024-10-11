# Adding a catalog into Trino

### Postgres example

[Postgres connector](https://trino.io/docs/current/connector/postgresql.html)

```sql
CREATE CATALOG example USING postgresql
WITH (
  "connection-url" = 'jdbc:pg:localhost:5432',
  "connection-user" = '${ENV:POSTGRES_USER}',
  "connection-password" = '${ENV:POSTGRES_PASSWORD}',
  "case-insensitive-name-matching" = 'true'
);
```

### Live Example

```sql
CREATE CATALOG <name-of-catalog> USING <connector-name>
WITH (
  "connection-url" = 'jdbc:postgresql://192.168.6.100:5432/70ld_db',
  "connection-user" = 'paul',
  "connection-password" = 'password'
);
```

- [create-catalog](https://trino.io/docs/current/sql/create-catalog.html#sql-create-catalog--page-root)
