# Using placeholders in database code

```sql
curs.execute(
    """
        INSERT INTO some_table (name, job_title, age)
        VALUES(%s, %s, %s);
    """,
    (
        'Paul', 'Senior Solutions Eng', 38
    )
)
```

The code you've provided is an example of how to execute an SQL query using
placeholders for parameters. This approach is used for several important
reasons:

1. **SQL Injection Prevention**: By using placeholders like `%s` instead of
   directly concatenating user input into the query string, you're protecting
   your code from SQL injection attacks. SQL injection occurs when malicious
   users manipulate input data to execute unintended SQL statements on your
   database. Using placeholders ensures that user input is treated as data, not
   executable code.

2. **Data Sanitization**: When you use placeholders and bind data values to
   them, the database driver takes care of properly sanitizing the input data.
   This prevents issues related to special characters, formatting, and escaping
   that can lead to errors or security vulnerabilities.

3. **Parameterized Queries**: This coding style is called parameterized queries.
   Parameterized queries improve the performance of your database interactions
   because the database can prepare and optimize the query execution plan before
   values are inserted. This can result in better query performance, especially
   for frequently executed queries.

4. **Code Readability**: Separating the query structure from the data values
   being inserted improves the readability of your code. Developers can easily
   understand what the query does without being distracted by the actual values.

5. Maintainability: Separating the query and data values makes it easier to
   modify the query if needed without affecting the way data is bound to it.
   This separation also makes it simpler to debug and maintain your code.

6. **Compatibility**: Different database systems might use different ways to
   represent placeholders (e.g., `%s` for PostgreSQL, `?` for SQLite, etc.), but
   the concept of parameterized queries is consistent across most database
   systems. This makes your code more portable and compatible with various
   databases.

7. **Consistency and Best Practices**: Using parameterized queries is considered
   a best practice in modern application development. It ensures secure and
   efficient database interactions.

Overall, writing queries with placeholders and binding data values to them is a
crucial practice to ensure the security, performance, and maintainability of
your database interactions. It's a fundamental technique that helps prevent
vulnerabilities and ensures the integrity of your application's data.
