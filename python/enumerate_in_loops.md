# Using enumerate for a counter in loops


```python
names = ['paul', 'fiona', 'charlie']
for i, names in enumerate(names, start=1):
    print(f"{i}: {names}")

# output ---
1: paul
2: fiona
3: charlie
```

When you use `enumerate()`, the function gives you back two loop variables:

- The count of the current iteration
- The value of the item at the current iteration

More information on using `enumerate()` can be found [here](https://realpython.com/python-enumerate/)
