# Handling a ZeroDivision Error

I was getting errors for `ZeroDivison` although I knew the sum was correct.

```python
pct = round((publisher_count - intermediary_count) / publisher_count, 2)
```

After some googling, to remove this error, I used a `try / except` which allowed me to resolve the error.

```python
try:
    pct = round((publisher_count - intermediary_count) / publisher_count, 2)
except ZeroDivisionError:
    pct = 0
```

#### exception ZeroDivisionError
Raised when the second argument of a division or modulo operation is zero. The associated value is a string indicating the type of the operands and the operation.
