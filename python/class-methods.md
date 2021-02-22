# Class methods

The [classmethod()](https://docs.python.org/3/library/functions.html#classmethod) method returns a class method for the given function. A class method is a method that is bound to a class rather than its object. Instead of accepting a `self` parameter, class methods take a `cls` parameter that points to the class—and not the object instance—when the method is called.

```python
class Employee:

    # Class variables
    raise_amount = 1.04

    def __init__(self, first...)
        self.first = first

    .....

    @classmethod
    def set_raise_amount(cls, amount):
        cls.raise_amount = amount

```

- A class method is a method which is bound to the class and not the instance of the class.
- They have the access to the state of the class as it takes a class parameter that points to the class and not the instance.
- It can modify a class state that would apply across all the instances of the class. For example it can modify a class variable that will be applicable to all the instances.

More information [here](https://github.com/mrpbennett/TIL/blob/master/python/instance_class_static_methods.md)
