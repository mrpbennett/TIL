# Checking for two strings in one col

The solution I provided earlier is specific to MS SQL (SQL Server). Let me show you how you can perform similar checks in **Presto/Trino** and **PostgreSQL** for finding two substrings in a column.

### 1. **Presto/Trino**

In Presto/Trino, you can use the `strpos()` function, which returns the position of a substring within a string (0 if not found). Here's how you can check for two substrings:

```sql
SELECT *
FROM your_table
WHERE strpos(your_column, 'substring1') > 0
  AND strpos(your_column, 'substring2') > 0;
```

- Replace `your_table` with the table name.
- Replace `your_column` with the column name.
- Replace `'substring1'` and `'substring2'` with the substrings you're searching for.

### 2. **PostgreSQL**

In PostgreSQL, you can use the `POSITION()` function or `strpos()` (just like in Presto/Trino). Here's an example using `POSITION()`:

```sql
SELECT *
FROM your_table
WHERE POSITION('substring1' IN your_column) > 0
  AND POSITION('substring2' IN your_column) > 0;
```

Or, you can use `strpos()` in PostgreSQL too:

```sql
SELECT *
FROM your_table
WHERE strpos(your_column, 'substring1') > 0
  AND strpos(your_column, 'substring2') > 0;
```

Both functions return the position of the substring, with `0` indicating the substring isn't found. Use whichever syntax you prefer depending on your SQL environment.

### 3. **MS SQL**

In SQL Server (MS SQL), you can check if a column contains two different substrings without using `LIKE '%something%'` by combining the conditions with a `CHARINDEX()` function. `CHARINDEX()` is used to find the starting position of a substring within a string, and if the result is greater than 0, the substring exists in the column.

Here's how you can check if a column contains two different substrings:

```sql
SELECT *
FROM your_table
WHERE CHARINDEX('substring1', your_column) > 0
  AND CHARINDEX('substring2', your_column) > 0;
```

In this query:

- Replace `your_table` with the table name.
- Replace `your_column` with the column name.
- Replace `'substring1'` and `'substring2'` with the substrings you're looking for.

This query will return rows where both substrings appear in the column. The `CHARINDEX()` function returns the position of the substring within the string, or 0 if it's not found.
