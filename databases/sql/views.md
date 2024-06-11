# What is a view and how to use one

A view in SQL is a virtual table that is based on the result-set of an SQL query. It contains rows and columns just like a real table, but it does not store the data itself. Instead, it dynamically retrieves the data whenever the view is queried. Views are a way to simplify complex queries, encapsulate business logic, and provide a level of abstraction from the underlying tables.

## How is a View Used?

1. **Simplifying Complex Queries**: Views can simplify complex SQL queries by encapsulating the logic in a reusable and manageable form. Instead of writing a complex query every time, you can write it once as a view and then query the view.

```sql
SELECT * FROM delivery_due_list;
```

2. **Security**: Views can provide a level of security by restricting access to specific columns or rows. For instance, you can create a view that only shows specific columns from a table, thus hiding sensitive data.

3. **Abstraction**: Views provide a way to present data in a different format than it is stored in the database. For example, you can create a view that joins multiple tables and presents a unified interface to the data.

4. **Data Consistency**: Views ensure that users see a consistent state of the data, especially useful in environments where the underlying tables are frequently updated.

## Example Usage

After creating the `delivery_due_list` view, you can use it in the same way as you would use a table. For example:

- To get all due deliveries:

```sql
SELECT * FROM delivery_due_list;
```

- To join with other tables:

```sql
SELECT d.id, d.customer_id, ddl.ship_date
FROM deliveries d
JOIN delivery_due_list ddl ON d.id = ddl.id;
```

In summary, the view `delivery_due_list` in your script simplifies the retrieval of delivery records that are due today or earlier, and views in general offer powerful ways to manage and interact with data in a database.

# Other handy user cases of using a view

**Simplifying Complex Queries**: When you frequently run complex queries with multiple joins, subqueries, or aggregations, a view can encapsulate this complexity. This allows you to reuse the complex query easily without rewriting it each time.

```sql
-- Complex query
SELECT o.order_id, c.customer_name, p.product_name
FROM orders o
JOIN customers c ON o.customer_id = c.id
JOIN products p ON o.product_id = p.id
WHERE o.order_date > '2023-01-01';

-- Simplified using a view
CREATE VIEW order_summary AS
SELECT o.order_id, c.customer_name, p.product_name
FROM orders o
JOIN customers c ON o.customer_id = c.id
JOIN products p ON o.product_id = p.id
WHERE o.order_date > '2023-01-01';

SELECT * FROM order_summary;
```

**Enhancing Security**: Views can restrict user access to specific columns or rows of data, providing a layer of security. For example, you can create a view that excludes sensitive columns from a table.

```sql
-- Original table with sensitive data
CREATE TABLE employees (
    id SERIAL,
    name VARCHAR(100),
    ssn CHAR(9), -- sensitive data
    salary DECIMAL(10, 2)
);

-- View excluding the sensitive data
CREATE VIEW employee_public AS
SELECT id, name, salary
FROM employees;

-- Users can query the view without accessing SSN
SELECT * FROM employee_public;
```

**Abstracting Data Structure Changes**: If the underlying database schema changes, views can help maintain consistency for applications and users. By modifying the view, you can shield users from changes in the table structure.

```sql
-- Original table structure
CREATE TABLE users (
    id SERIAL,
    fullname VARCHAR(100),
    birthdate DATE
);

-- Application uses this view
CREATE VIEW user_info AS
SELECT id, fullname, birthdate
FROM users;

-- Table structure changes
ALTER TABLE users
RENAME COLUMN fullname TO name;

-- Adjust the view to reflect the changes
CREATE OR REPLACE VIEW user_info AS
SELECT id, name AS fullname, birthdate
FROM users;
```

**Improving Readability and Maintenance**: Views can provide meaningful names and simplify access to data, improving code readability and maintainability. This is particularly useful in large and complex databases.

```sql
-- Without view: complex query with cryptic table and column names
SELECT a.col1, b.col2
FROM tbl_a a
JOIN tbl_b b ON a.id = b.a_id
WHERE a.col3 > 100;

-- With view: simplified and meaningful
CREATE VIEW sales_summary AS
SELECT a.id, a.amount, b.region
FROM sales a
JOIN regions b ON a.region_id = b.id;

SELECT * FROM sales_summary WHERE amount > 100;
```

**Aggregating Data**: Views can be used to pre-aggregate data, making it faster and easier to retrieve summary information.

```sql
-- Aggregate data view
CREATE VIEW monthly_sales AS
SELECT EXTRACT(MONTH FROM sale_date) AS month, SUM(amount) AS total_sales
FROM sales
GROUP BY EXTRACT(MONTH FROM sale_date);

-- Retrieve aggregated data
SELECT * FROM monthly_sales;
```

**Consistent Data Presentation**: When you need to present data in a specific format consistently, views can ensure that the data is always presented in the same way, regardless of how the underlying data is stored.

```sql
-- Consistent presentation of data
CREATE VIEW product_overview AS
SELECT id, name, price, 'USD' AS currency
FROM products;

SELECT * FROM product_overview;
```

In summary, views are a powerful tool in SQL databases that can simplify complex queries, enhance security, provide abstraction from data structure changes, improve readability and maintenance, aggregate data, and ensure consistent data presentation.
