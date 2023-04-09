# Defining a FK in Postgres when creating a table

In PostgreSQL, the most effective way to define a foreign key when creating a
table is to use the "REFERENCES" clause in the column definition.

Here's an example:

```sql
CREATE TABLE orders (
  order_id SERIAL PRIMARY KEY,
  customer_id INTEGER REFERENCES customers(customer_id),
  order_date DATE
);
```

In this example, the `customer_id` column is defined as a foreign key that
references the `customer_id` column in the `customers` table.

The "REFERENCES" clause specifies the name of the referenced table and the name
of the referenced column. It also ensures that referential integrity is
maintained, meaning that a record cannot be deleted from the referenced table if
there are still records in the table with foreign key references to it.

By defining the foreign key at the column level, you can ensure that the
relationship between the two tables is clear and explicit. This can help improve
the performance and reliability of your database operations.
