# Returning data and col names in Presto Py

Before I found out about `cur.description` I wasn't returning column names in my queries. This is how I now return col names from presto python. I return a dict with keys `data` and `cols`. The data returns `curs.fetchall()` and cols returns `cur.description`

```python
def run_presto(query):
    with prestodb.dbapi.connect(
        host="some_host",
        port=8443,
        user="xxx",
        catalog="xxx",
        schema="xxx",
        http_scheme="https",
        auth=prestodb.auth.BasicAuthentication("xxx", "xxx"),
    ) as conn:
        cur = conn.cursor()
        cur.execute(query)

        return {"data": cur.fetchall(), "cols": cur.description}
```

Then the function is called like this. I then generally put everything into a DataFrame. We have to use list comprehension to get what we want from `cur.description` as the col name is the first item in the list that is returning from `cur.description`

```python
data = run_presto(
"""
select * from some_table;
"""
)


df = pd.DataFrame(data['data'], columns=[i[0] for i in data['cols']])
```
