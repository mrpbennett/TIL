# map() Function

The `map()` function executes a specified function for each item in a iterable. The item is sent to the function as a parameter.

```python
map(<function>, <iterables>)
```

**function** - Required. The function to execute for each item
**iterable** - Required. A sequence, collection or an iterator object. You can send as many iterables as you like, just make sure the function has one parameter for each iterable.

### Example

Make new fruits by sending two iterable objects into the function:

```python
def myfunc(a, b):
  return a + b

x = map(myfunc, ('apple', 'banana', 'cherry'), ('orange', 'lemon', 'pineapple'))
```

Output - `['appleorange', 'bananalemon', 'cherrypineapple']`

`map()` can be used instead of list comprehension.

```python
def double(x):
    return x * 2

numbers = [1,2,3,4,5]
doubled = [double(x) for x in numbers] # list comprehension
double = map(double, numbers) # using the map() function

# both will give the same output
```
