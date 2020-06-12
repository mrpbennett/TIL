# Dictionaries

A dictionary is a collection which is unordered, changeable and indexed. In Python dictionaries are written with curly brackets, and they have keys and values.

```python
car = {
    "brand": "Ford",
    "model": "Mustang",
    "year": 1964
}
```

You can access the items of a dictionary by referring to its key name, inside square brackets:

```python
x = car['brand']
print(x)
```

There is also a method called `.get()` that will give you the same result:

```python
y = car.get('model')
print(y)
```

---

You can also access a list of dicts by using subscript notation and then using the `.get()` method.

```python
friends = [
    {'name': 'Dave', 'age': 35, 'job': 'MSFT'},
    {'name': 'Dale', 'age': 35, 'job': 'bread'},
    {'name': 'Martyn', 'age': 41, 'job': 'none'}
]

x = friends[2].get('job') 
print(x)
```


### Loop Through a Dictionary
You can loop through a dictionary by using a for loop.

When looping through a dictionary, the return value are the keys of the dictionary, but there are methods to return the values as well.

```python
thisdict = {
    "brand": "Ford",
    "model": "Mustang",
    "year": 1964
}

# Print all key names in the dictionary, one by one:
for x in thisdict:
    print(x)

# Print all values in the dictionary, one by one:
for x in thisdict:
    print(thisdict[x])

# You can also use the `.values()` method to return values of a dictionary:
for x in thisdict.values():
    print(x)

# Loop through both keys and values, by using the items() method:
for x, y in thisdict.items():
    print(x, y)
```


### Check if Key Exists
To determine if a specified key is present in a dictionary use the in keyword:

```python
thisdict = {
    "brand": "Ford",
    "model": "Mustang",
    "year": 1964
}
if "model" in thisdict:
    print("Yes, 'model' is one of the keys in the thisdict dictionary")
```