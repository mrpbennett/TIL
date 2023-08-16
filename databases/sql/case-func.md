# `CASE` function

The `CASE` function in SQL is a powerful and flexible way to perform conditional
logic within your queries. It allows you to define conditions and specify
corresponding actions or values based on those conditions. The `CASE` function
can be used in various parts of a SQL query, such as in the `SELECT` statement,
in the `WHERE` clause, or even in the `ORDER BY` clause.

The basic syntax of the `CASE` function is as follows:

```sql
CASE
    WHEN condition_1 THEN result_1
    WHEN condition_2 THEN result_2
    ...
    ELSE default_result
END
```

Here's how it works:

- `WHEN`: You specify conditions that you want to test. If a condition evaluates
  to true, the corresponding result is returned.
- `THEN`: This specifies the value or expression to return if the condition is
  true.
- `ELSE`: This is optional. If none of the previous conditions are true, the
  ELSE block provides a default value or action.

Let's look at some examples to illustrate how to use the `CASE` function:

### Example 1: Using `CASE` in `SELECT` statement

Suppose you have a table "employees" and you want to categorize employees based
on their salary into different salary ranges.

```sql
SELECT
    employee_name,
    salary,
    CASE
        WHEN salary >= 50000 AND salary < 70000 THEN 'Low Range'
        WHEN salary >= 70000 AND salary < 90000 THEN 'Mid Range'
        ELSE 'High Range'
    END AS salary_range
FROM employees;
```

### Example 2: Using `CASE` in `WHERE` clause

Suppose you want to filter out orders that were placed before a certain date,
and for orders without a specific date, you want to consider them as valid.

```sql
SELECT *
FROM orders
WHERE CASE
    WHEN order_date IS NULL THEN 1
    WHEN order_date >= '2023-01-01' THEN 1
    ELSE 0
END = 1;
```

### Example 3: Using `CASE` in `ORDER BY` clause

Suppose you want to order products based on their availability status.

```sql
SELECT product_name, stock_quantity
FROM products
ORDER BY CASE
    WHEN stock_quantity > 0 THEN 0
    ELSE 1
END, product_name;
```

In this example, products with higher stock quantities (available) will appear
first in the result set.

The `CASE` function can be very versatile and is often used to handle more
complex conditional scenarios within SQL queries. It's important to note that
the exact syntax might vary slightly depending on the SQL database system you
are using, but the fundamental principles of the `CASE` function remain
consistent.
