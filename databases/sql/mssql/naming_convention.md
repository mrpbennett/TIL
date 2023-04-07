# Naming convention for MS SQL

The most common naming convention for Microsoft SQL Server (MSSQL) is to use
PascalCase for object names, which means that the first letter of each word is
capitalized and there are no spaces or underscores between words. For example, a
table name might be `UserAccounts` or a column name might be `FirstName`.

Here are some naming conventions commonly used in MSSQL:

Table names: Use a singular noun or a noun phrase to name tables. For example,
`User`, `Order`, or `CustomerAccount`.

Column names: Use a singular noun or a noun phrase to name columns. For example,
`Id`, `FirstName`, or `OrderDate`.

Primary keys: Name primary key columns `Id` or `<TableName>Id`, where
`<TableName>` is the name of the table. For example, a primary key column for a
table named `User` might be named `Id` or `UserId`.

Foreign keys: Name foreign key columns `<ReferencedTable>Id` or `<TableName>Id`,
where `<ReferencedTable>` is the name of the table being referenced. For
example, a foreign key column in a table named Order that references a table
named Customer might be named `CustomerId` or `Customer_Id`.

Constraints: Name constraints using a prefix that indicates the type of
constraint, followed by a brief description of the constraint. For example, a
unique constraint on a column named `Email` might be named `UQ_User_Email`.

**Again, naming conventions are not set in stone, and you should choose the
convention that works best for your project and team. The most important thing
is to be consistent with your naming throughout your database schema.**
