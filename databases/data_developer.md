# Data Developer Journey

This is my Data dev journy....

## Junior

### Fundamental SQL concepts

The table that I will be using here:

#### users_table

| id  | name         | email              |
| --- | ------------ | ------------------ |
| 1   | John Doe     | john@example.com   |
| 2   | Jane Smith   | jane@example.com   |
| 3   | Robert Brown | robert@example.com |

#### SELECT

```sql
SELECT id, name
FROM users_table
WHERE name like 'J%';
```

The above query would return

| ID  | Name       |
| --- | ---------- |
| 1   | John Doe   |
| 2   | Jane Smith |

#### INSERT

```sql
INSERT INTO users_table (id, name, email)
VALUES (4, 'Jonny Appleseed', 'jonny@example.com');
```

The above would make the table as follows

| id  | name            | email              |
| --- | --------------- | ------------------ |
| 1   | John Doe        | john@example.com   |
| 2   | Jane Smith      | jane@example.com   |
| 3   | Robert Brown    | robert@example.com |
| 4   | Jonny Appleseed | jonny@example.com  |

#### UPDATE

```sql
UPDATE users_table
SET name = 'Joe Bloggs', email = 'joe@example.com'
WHERE id = 4
```

This would return the following table as I have changed the name of user id 4

| id  | name         | email              |
| --- | ------------ | ------------------ |
| 1   | John Doe     | john@example.com   |
| 2   | Jane Smith   | jane@example.com   |
| 3   | Robert Brown | robert@example.com |
| 4   | Joe Bloggs   | joe@example.com    |

#### DELETE

```sql
DELETE FROM users_table where id = 4
```

This would return, as we have removed user with id 4 from the table.

| id  | name         | email              |
| --- | ------------ | ------------------ |
| 1   | John Doe     | john@example.com   |
| 2   | Jane Smith   | jane@example.com   |
| 3   | Robert Brown | robert@example.com |

### Database Basics

## Mid-Level

## Lead
