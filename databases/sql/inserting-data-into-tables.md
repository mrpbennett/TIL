# How to insert data into tables.

Lets say we have a simple table within our db for students. This would be created as follows:

```sql
CREATE TABLE student(
    id INT PRIMARY KEY,
    name VARCHAR(20),
    email VARCHAR(30) DEFAULT 'no email',
    major VARCHAR(20)
);
```

Now the table has been created we need to insert data into it. We do that as follows:

```sql
INSERT INTO student VALUES(1, 'Paul Bennett', 'pbennett.uk@gmail.com', 'SQL Databases');
```

Let me break down what's in the value `()`

-   1 = id
-   Paul Bennett = name
-   pbennett.uk@gmail.com = email
-   SQL Databases = major

We insert the data in the order of the columns.
