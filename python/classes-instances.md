# Classes

Classes provide a means of bundling data and functionality together. Creating a new class creates a new *type* of object

```Python
class Employee:

    def __init__(self, first, last, pay):
        self.first = first
        self.last = last
        self.pay = pay
        self.email = f'{first}.{last}@company.com'

    def fullname(self):
        return f'{self.first} {self.last}'


emp1 = Employee('Paul', 'Bennett', 154000)

```



When creating a methods within a class, they receive the instance as the first argument automatically which should be called `self` after this, you can add any other arguments to accept. The first method `__init__()` is a special method, which is called class constructor or initialisation method that Python calls when you create a new instance of this class.

```Python
class ClassName:
    def __init__(self, ...)
```

# Instances

To create instances of a class, you call the class using class name and pass in whatever arguments its `__init__` method accepts.

```Python
"This would create first object of Employee class"
emp1 = Employee("Zara", 2000)
```
