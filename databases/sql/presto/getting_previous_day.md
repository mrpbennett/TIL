# Getting previous day

Just a simple snippet on getting previous days in Presto

```sql
DATE(day) = CURRENT_DATE - INTERVAL '1' day
```

For this snippet depending on how the data type is assigned, in our tables we have `day` as a `varchar` therefore for this to work I have to cast `day`

As per the docs on [Date and Time function operators](https://prestodb.io/docs/current/functions/datetime.html#date)

`date(x) → date` is an alias for `CAST(x AS date)`
