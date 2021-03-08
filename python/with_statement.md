# with statement in Python

`with` statement in Python is used in exception handling to make the code cleaner and much more readable. It simplifies the management of common resources like file streams. Observe the following code example on how the use of with statement makes code cleaner.

### File Handling
```python

# 1) without using with statement 
file = open('file_path', 'w') 
file.write('hello world !') 
file.close() 
  
# 2) without using with statement 
file = open('file_path', 'w') 
try: 
    file.write('hello world') 
finally: 
    file.close()
```

### Using `with` statment
```python
with open('file_path', 'w') as file: 
    file.write('hello world !') 
```

Notice that unlike the first two implementations, there is no need to call file.close() when using with statement. The with statement itself ensures proper acquisition and release of resources. An exception during the file.write() call in the first implementation can prevent the file from closing properly which may introduce several bugs in the code, i.e. many changes in files do not go into effect until the file is properly closed.

### Other methods

`with` can also be used to close db connections:

```python
def connect_to_presto():
    with prestodb.dbapi.connect(
        host=db_host,
        port=8443,
        user="pbennett",
        catalog="gridhive",
        schema="rpt",
        http_scheme="https",
        auth=prestodb.auth.BasicAuthentication("pbennett", db_pwd),
    ) as conn:
        with conn.cursor() as cur:
            cur.execute("select * from some.table")
            rows = cur.fetchall()
            return rows
```

more information [here](https://www.geeksforgeeks.org/with-statement-in-python/)