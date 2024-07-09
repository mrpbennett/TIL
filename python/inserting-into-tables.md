# Inserting into tables in Python the RIGHT way.

This is the most effective way to insert data into a table. The way this is done, means you don't have to worry about the query its self. As all the columns and placeholders and automatically inserted into the query with the following logic.

```python
curs.execute(
    f"""
        INSERT INTO garmin.sleep ({', '.join(columns)})
        VALUES ({', '.join(['%s'] * len(values))})
    """,
    tuple(values),
)
```

Here we're joining the list of `columns`, then using the length of `columns` to count how many place holders are required.

```python
def somefunc(data) -> bool:

    row_data = {
        "uuid": data['uuid'],
        "name": data['name'],
        "email": data['email']
    }

    columns = [
        "uuid",
        "name",
        "email",
    ]

    values = [row_data[col] for col in columns]

    try:
        with db as conn:
            with conn.cursor() as curs:
                curs.execute(
                    f"""
                        INSERT INTO garmin.sleep ({', '.join(columns)})
                        VALUES ({', '.join(['%s'] * len(values))})
                    """,
                    tuple(values),
                )

        return True

    except KeyError as ke:
        logging.error(f"Missing key in data: {ke}")
    except Exception as e:
        logging.error(f"Error inserting data: {e}")

    return False
```

This allows the ease of use when it comes to creating the query and not missing any columns or placeholders before insertion.

In simple terms this is what happens

```python
row_data = {
        "uuid": 1234567890,
        "name": 'Paul',
        "email": 'pbennett@domain.com'
    }

columns = [
    "uuid",
    "name",
    "email",
]
values = [row_data[col] for col in columns]


x = {', '.join(columns)}
v = {', '.join(['%s'] * len(values))}

print(x) # {'uuid, name, email'}
print(v) # {'%s, %s, %s'}
```
