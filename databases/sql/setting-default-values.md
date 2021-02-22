# How to set a set default value

Sometimes we want to set a default value into our tables, if there is no data for that column.

```sql
email VARCHAR(30) DEFAULT 'no email'
```

Using `DEFAULT` after we have declared the data type allows us to use a default value.
