# Using `regexp_like()`

Using multiple `LIKE` statements is just slow and annoying...

Welcome [`regexp_like()`](https://prestodb.io/docs/current/functions/regexp.html?highlight=regexp_like)

This function allows you to use reg expression to use many LIKE statements in one line.

### With LIKE

```sql
select * from my_table where my_column LIKE ['%hello%'|'example%'|'%random%'|'%demo'];
```

### With regexp_like()

```sql
select *
from my_table
where regexp_like(my_column, 'hello|example|random|demo');
```

### Note..

the `|` is OR
