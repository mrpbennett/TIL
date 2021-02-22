# Auto incrementing IDs

Sometimes we had to add a lot of data into our tables, each row should have a primary key. This can be a manual task. There is a way to always auto increment that number and let the db handle it.

```sql
create table contacts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) not null,
    email VARCHAR(30),
    telephone VARCHAR(30)
);
```

If we look the above table, we can see that our `id INT PRIMARY KEY` has `AUTO_INCREMENT` which means this will increment the ID automatically.

To add the data we would do the following:

```sql
insert into contacts(name, email, telephone) values('Paul Bennett', 'pbennett.uk@gmail.com', '07551 123456');
```
