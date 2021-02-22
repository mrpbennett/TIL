# Dunder Mehtods

These two 'dunder' methods for classes allow you to change how you reper

The `__str__` method returns a string output “ Paul, is 36 years old” instead of the object id in the memory, whenever we call the class. However, if you just inspect the object, you will still get the memory address.

 
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    # __str__ allows you to convert Python objects into strings
    def __str__(self):
        return f"{self.name}, is {self.age} years old"

    # __repr__ allows you to return unambiguous version of your class
    def __repr__(self):
        return f"<{self.name}, is {self.age} years old>"


paul = Person("Paul", 36)
print(paul)  # returns : Paul, is 36 years old
```

### Another Example

```python
import datetime

today = dataetime.date.today()

str(today) # '2021-02-22'
repr(today) # 'datetime.date(2021, 02, 22)
```


`__str__` or `str()` is used for creating output that is human-readable are must be for end-users. But, `repr()` or `__repr__` must serve the purpose of debugging and development
