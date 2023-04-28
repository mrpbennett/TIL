# CAST Function

CAST is a function in SQL that converts an expression of one data type to another data type. It is used when you need to change the data type of a value, expression, or column to a different data type.

Here is the syntax of the CAST function:

```sql
CAST(expression AS data_type)
```

In this syntax, expression is the value or expression that you want to convert, and data_type is the target data type that you want to convert the expression to.

For example, if you have a column in a table with data type VARCHAR and you want to convert it to INT, you can use the CAST function like this:

```sql
SELECT CAST(column_name AS INT) AS new_column_name FROM table_name;

-- OR

CAST(SUM(platformfeecpm)/1000 AS decimal(10,2)) platformfee,
```

This query will return a new column named "new_column_name" that contains the values of the "column_name" column, converted to the INT data type.

Note that when casting values, it is important to ensure that the source value can be successfully converted to the target data type. Otherwise, the CAST function will generate an error. Additionally, when casting values with a size limit, it is important to make sure that the target data type can accommodate the full size of the source value.