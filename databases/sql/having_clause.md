# HAVING clause

In SQL, the `HAVING` clause is used in conjunction with the `GROUP BY` clause to
filter the results of a query based on the aggregated values of grouped rows. It
allows you to apply a condition to groups of rows created by the `GROUP BY`
clause. In essence, the `HAVING` clause works similar to the WHERE clause, but
it operates on grouped data rather than individual rows.

Here's the basic syntax of using the `HAVING` clause:

```sql
SELECT column1, aggregate_function(column2)
FROM table
GROUP BY column1
HAVING condition;
```

In this syntax:

- `column1` is a column by which you are grouping the data using the `GROUP BY`
  clause.
- `aggregate_function(column2)` is an aggregate function applied to `column2`
  within each group, such as `SUM`, `COUNT`, `AVG`, etc.
- `condition` is the filtering condition applied to the aggregated groups using
  the `HAVING` clause.

Here's an example to illustrate how the `HAVING` clause works:

Suppose you have a table named sales with columns region, product, and amount.
You want to find regions where the total sales amount is greater than 1000. You
can use the `HAVING` clause as follows:

```sql
SELECT region, SUM(amount) as total_sales
FROM sales
GROUP BY region
HAVING SUM(amount) > 1000;
```

In this example, the `GROUP BY` clause groups the data by region, and the
`HAVING` clause filters the groups based on the total sales amount
`(SUM(amount))` being greater than 1000.

Key points to remember about the `HAVING` clause:

- The `HAVING` clause is used with aggregated functions like `SUM`, `COUNT`,
  `AVG`, etc. It operates on the result set after the `GROUP BY` clause has
  grouped the data.
- The `HAVING` condition is evaluated for each group, and only groups that
  satisfy the condition are included in the final result.
- If you want to filter individual rows before grouping, you should use the
  `WHERE` clause.

In summary, the `HAVING` clause allows you to filter the results of a grouped
query based on aggregated values, providing a way to perform conditional
filtering on groups of data.
