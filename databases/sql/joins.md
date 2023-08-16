# `JOIN` in SQL

In SQL, a join is a operation that combines rows from two or more tables based
on a related column between them. Joins are used to retrieve data from multiple
tables and create a single result set that contains information from all the
tables involved.

There are several types of joins in SQL:

**INNER JOIN**: An inner join returns only the rows where there is a match in
both tables. It combines rows from both tables based on the specified condition.

```sql
SELECT columns
FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```

**LEFT JOIN (or LEFT OUTER JOIN)**: A left join returns all the rows from the
left table and the matched rows from the right table. If there's no match in the
right table, the result will contain null values for the right table's columns.

```sql
SELECT columns
FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```

**RIGHT JOIN (or RIGHT OUTER JOIN)**: A right join is similar to a left join,
but it returns all the rows from the right table and the matched rows from the
left table. If there's no match in the left table, the result will contain null
values for the left table's columns.

```sql
SELECT columns
FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```

**FULL JOIN (or FULL OUTER JOIN)**: A full join returns all the rows from both
tables, along with the matched rows. If there's no match in either table, the
result will contain null values for the columns from the table with no match.

```sql
SELECT columns
FROM table1
FULL JOIN table2 ON table1.column = table2.column;
```

**CROSS JOIN**: A cross join returns the Cartesian product of the two tables,
which means all possible combinations of rows from both tables. It does not
require a specific condition.

```sql
CROSS JOIN: A cross join returns the Cartesian product of the two tables, which means all possible combinations of rows from both tables. It does not require a specific condition.
```

When using joins, you specify the columns to join on using the ON keyword. The
columns being compared should have the same data type or be compatible. Joins
are powerful tools for combining data and extracting meaningful insights from
multiple related tables.

Here's a simplified example using the `INNER JOIN`:

Suppose you have two tables: "Customers" and "Orders." You want to retrieve a
list of customers and their orders.

```sql
SELECT Customers.CustomerName, Orders.OrderDate
FROM Customers
INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

This query uses an `INNER JOIN` to combine the "Customers" and "Orders" tables
based on the "CustomerID" column, and it retrieves the customer name and order
date for each matched row.

# ELI5 Version

Imagine you have two tables, like big spreadsheets, full of information. One
table has names of people and their ages, and the other table has names of
people and their favorite colors. You want to put together a new list that shows
people's names along with their ages and favorite colors, all in one place.

Here's how you'd do it with SQL joins:

- **Inner Join**: It's like finding friends who are in both your sports club and
  your music club. You take the names that appear in both lists and make a new
  list showing their names, ages, and favorite colors.

- **Left Join**: Imagine you have a list of all your friends and their favorite
  foods, and you want to see if any of your friends are in a cooking class. You
  make a list of all your friends, and if any are in the cooking class, you also
  note their favorite foods. If they're not in the cooking class, you leave that
  part blank.

- **Right Join**: This is like the previous example, but you start with a list
  of people from the cooking class and see if any of them are your friends. You
  include their favorite foods if they are your friends, and you might have some
  empty spots if they're not.

- **Full Join**: Imagine you have a list of students who play soccer and another
  list of students who play basketball. A full join would give you a new list
  with all the students who play either soccer or basketball, along with their
  favorite snacks. If a student only plays one sport, you still include them,
  and you leave a space for the favorite snack of the sport they don't play.

- **Cross Join**: Think of it like matching every person from one list with
  every person from the other list. If you have a list of boys and a list of
  girls, a cross join would show you all possible combinations of a boy and a
  girl, even if they're not actually friends or related.

In these examples, the tables are like your spreadsheets, and the join types
help you combine the information in different ways to create a new combined list
with more details. SQL joins work similarly, allowing you to bring together data
from different tables based on shared information, like names, IDs, or other
common values.
