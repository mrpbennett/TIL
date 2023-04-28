# Decimal DataType

In SQL, the decimal data type is used to store fixed-point numbers with a specific precision and scale. The precision determines the total number of digits that can be stored in the number, while the scale determines the number of digits that can be stored to the right of the decimal point.

Here's the syntax for the decimal data type in SQL:

```sql
DECIMAL(precision, scale)
```

In this syntax, precision is the total number of digits that can be stored in the number, and scale is the number of digits that can be stored to the right of the decimal point.

For example, if you have a decimal column in a table that stores a monetary value with a precision of 10 and a scale of 2, you can declare the column like this:

```sql
CREATE TABLE MyTable (
  MyDecimalColumn DECIMAL(10, 2)
);
```

This will create a table named "MyTable" with a column named "MyDecimalColumn" that can store fixed-point numbers with up to 10 digits, and up to 2 of those digits to the right of the decimal point.

When inserting or updating values in a decimal column, you must ensure that the value you are inserting or updating is within the specified precision and scale of the column. If the value exceeds the precision or scale of the column, an error will be generated.

Overall, the decimal data type is commonly used in SQL for storing monetary values and other fixed-point numbers that require a specific level of precision and scale.