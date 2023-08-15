# Counting uniques in SQL

To count unique values in a SQL query, you can use the `COUNT(DISTINCT column)`
function. This function counts the distinct (unique) values in the specified
column of your table. Here's the basic syntax:

```sql
SELECT COUNT(DISTINCT column_name) AS unique_count
FROM table_name;
```

For example, let's say you have a table named "orders" with a column named
"customer_id," and you want to count the number of unique customer IDs:

```sql
SELECT COUNT(DISTINCT customer_id) AS unique_customer_count
FROM orders;
```

Remember that the `COUNT(DISTINCT column)` function works within the context of
the selected rows. If you have additional conditions or joins in your query, the
count will be based on the distinct values within those results.
