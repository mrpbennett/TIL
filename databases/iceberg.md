# Iceberg Tables

Apache Iceberg is an open table format for huge analytic datasets. It is designed to work with big data engines like Apache Spark, Trino, and Hive, and supports features like schema evolution, partitioning, and time travel.

## What are Iceberg Tables?

Iceberg tables are a type of table format that provides advanced capabilities for managing large-scale datasets. They allow for efficient querying, updates, and maintenance of data. Iceberg tables are particularly useful for data lakes, where data is stored in a distributed file system.

### Key Features:

- **Schema Evolution**: Modify table schemas without rewriting data.
- **Partitioning**: Organize data for efficient querying.
- **Time Travel**: Query historical versions of data.
- **ACID Transactions**: Ensure data consistency.

## Creating Iceberg Tables

To create an Iceberg table, you can use SQL commands. Below is an example:

```sql
CREATE TABLE my_iceberg_table (
    id BIGINT,
    name STRING,
    created_at TIMESTAMP
) USING iceberg
PARTITIONED BY (created_at);
```

### Explanation:

- `USING iceberg`: Specifies the Iceberg table format.
- `PARTITIONED BY (created_at)`: Partitions the table by the `created_at` column.

## What is a Partition?

A partition is a way to divide a table into smaller, manageable pieces based on the values of one or more columns. For example, you can partition a table by date, region, or any other column.

### Why Partition a Table?

Partitioning improves query performance by reducing the amount of data scanned. For instance, if a table is partitioned by date, a query filtering by a specific date will only scan the relevant partition instead of the entire table.

### Example Query on a Partitioned Table:

```sql
SELECT * FROM my_iceberg_table
WHERE created_at = '2025-07-03';
```

This query will only scan the partition corresponding to `2025-07-03`, making it faster and more efficient.
