# End parameter in `print()`

By default python’s `print()` function ends with a newline. Python’s print() function comes with a parameter called ‘end’. By default, the value of this parameter is ‘\n’, i.e. the new line character. You can end a print statement with any character/string using this parameter.

```python
n = 5

for i in range(n):
    print(i + 1, end="") # would return 12345
```
