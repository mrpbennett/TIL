# \*args and \*\*kwargs

\*args and \*\*kwargs allow you to pass multiple arguments or keyword arguments to a function. Consider the following example.

## \*args

This is a simple function that takes two arguments and returns their sum:

```python
def my_sum(a, b):
    return a + b
```

The above function is limited to only **two** arguments, you're unable to use more than two.

```python
# sum_integers_args.py
def my_sum(*args):
    result = 0
    # Iterating over the Python args tuple
    for x in args:
        result += x
    return result

print(my_sum(1, 2, 3))
```

In the above example, you’re no longer passing a list to my_sum(). Instead, you’re passing three different positional arguments. my_sum().

## \*\*kwargs

\**kwargs works just like *args, but instead of accepting positional arguments it accepts keyword (or named) arguments. As per this example:

```python
# concatenate.py
def concatenate(**kwargs):
    result = ""
    # Iterating over the Python kwargs dictionary
    for arg in kwargs.values():
        result += arg
    return result

print(concatenate(a="Real", b="Python", c="Is", d="Great", e="!"))
```
