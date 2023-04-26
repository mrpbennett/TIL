# COALESCE useage in MS SQL

COALESCE is a function in Microsoft SQL Server (MS SQL) that returns the first non-null expression among its arguments. It takes two or more expressions as its arguments and returns the value of the first expression that is not null.

Here is the syntax of the COALESCE function:

```sql
COALESCE(expression1, expression2, ..., expression_n)
```

In this syntax, expression1, expression2, ..., expression_n are the expressions to be evaluated. The COALESCE function returns the value of the first expression that is not null. If all expressions are null, the function returns null.

Here's an example of how to use the COALESCE function in MS SQL:

```sql
SELECT COALESCE(NULL, 2, 3) AS Result;
```

In this example, the COALESCE function returns the first non-null value from its arguments, which is 2. If the first argument had not been null, the function would have returned its value instead.