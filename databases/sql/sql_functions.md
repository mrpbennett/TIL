# SQL functions in Supabase

Postgres has built-in support for
[SQL functions](https://www.postgresql.org/docs/current/sql-createfunction.html).
These functions live inside your database.

Below is a SQL function that allows me to sum up the `earnings` col in one of my
tables. Supabase doesn't provide a `sum` feature, therefore it's recommended
that you write a SQL function to get the desired result.

Writing the function in the manner below allows you to use SQL like you would in
say DataGrip. This function can be called again and again within your code base.

```sql
CREATE OR REPLACE function TopEarners()
RETURNS TABLE( "firstName" text, "lastName" text, earnings int)
AS $$

SELECT "firstName", "lastName", SUM("earnings")
FROM "PPStravaActivities"
GROUP BY 1,2
ORDER BY 3 DESC;

$$ language sql;
```

As we know functions are designed to be used for repeatable code, the above
isn't repeatable I just needed a way to get what I needed from my table. A good
use of an SQL function, would be to insert something into a table. This would
prevent you from writing yout SQL again, and again. Instead you could pass
arguments into your SQL function instead.

More info on SQL functions in
[supabase](https://supabase.com/docs/guides/database/functions)
