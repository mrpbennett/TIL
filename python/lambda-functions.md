# Lambda Functions

A lambda function doesnt have a name and only used to return values.

```python
lambda <parameter_list>: <expression>`
```

The power of lambda is better shown when you use them as an anonymous function inside another function.

```python
# normal func
def add(x, y):
    return x + y


# lambda func
lambda x, y: x + y

# lambda within a function
def myfunc(n):
  return lambda a : a * n

mydoubler = myfunc(2)

print(mydoubler(11))
```
