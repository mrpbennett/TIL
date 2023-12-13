# COALESCE function

The COALESCE function in SQL is used to return the first non-`NULL` value in a
list of arguments. It's very useful for dealing with `NULL` values in your
database.

Here's a basic outline of how COALESCE works:

- **Syntax**: `COALESCE(value1, value2, ..., valueN)`
- **Description**: This function goes through the values in the order they are
  given and returns the first one that is not `NULL`.
- **Use Case**: It's often used in data reporting and manipulation to substitute
  `NULL` values with a default value, making the data more readable or ensuring
  that operations like concatenation or arithmetic don't fail due to `NULL`
  values.

### Example Usage

Imagine you have a table named Employees with columns `FirstName`, `MiddleName`,
and `LastName`. Some employees might not have a middle name, so `MiddleName`
could be `NULL` for some records.

If you want to create a full name for each employee, you might want to use
COALESCE to handle `NULL` middle names:

```sql

SELECT
    FirstName,
    COALESCE(MiddleName, '') as MiddleName,
    LastName,
    FirstName || ' ' || COALESCE(MiddleName, '') || ' ' || LastName AS FullName
FROM Employees;
```

In this example, if MiddleName is `NULL`, it will be replaced with an empty
string, allowing the concatenation to proceed without resulting in `NULL` for
the entire FullName.

### Points to Note

1. **If All Arguments are** `NULL`: If all the values you pass to `COALESCE` are
   `NULL`, the result is `NULL`.
2. **Type Compatibility**: The values you provide to `COALESCE` should generally
   be of compatible types, as the function returns a value with the same type as
   the input arguments.
3. **Performance**: While `COALESCE` is a simple and powerful function, always
   consider the impact on performance, especially in complex queries or when
   working with large datasets.

The `COALESCE` function simplifies handling `NULL` values and is supported in
most SQL database systems, including MySQL, PostgreSQL, SQL Server, and others.
