# Class Inheritance

Class inheritance allows other classes to use methods from another class. We have `Device` as our base class, then we have `Printer` which inherits from Device.

```python
class Printer(Device):
    def __init__(self, name, connected_by, capacity):
        super().__init__(name, connected_by)
```

Using the above we can inherit from `Device` for our Printer class. We use `super().__init__()` which gets the parent class and calls the `__init__` method. 

**Remember** the `Device` class doesn't know anything about the `Printer` class. However the `Printer` class is related to the `Device` class, it knows about it, it's the child of `Device`


```python
class Device:
    def __init__(self, name, connected_by):
        self.name = name
        self.connected_by = connected_by
        self.connected = True

    def __str__(self):
        return f"Device {self.name!r} ({self.connected_by})"

    def disconnected(self):
        self.connected = False
        print("Disconnected!")


# new class which inherits all Device's methods
class Printer(Device):
    def __init__(self, name, connected_by, capacity):
        super().__init__(name, connected_by)
        self.capcity = capacity
        self.remaining_pages = capacity

    def __str__(self):
        return f"{super().__str__()} ({self.remaining_pages} pages remaining)"

    def print(self, pages):
        if not self.connected:
            print('your printer is not connected!')
            return
        print(f'printing {pages} pages.')
        self.remaining_pages -= pages
```

The child class can still gain access to the parents methods if called on the child class.

```python
printer = Printer('HP Smart', 'USB' 500) # instance of Printer
printer.disconnected() # same instance using `Device.disconnected()`
```

This is possible, because Python will attempt to look for the `disconnected` method within the `Printer` class. If it's unable to find it there, Python will then go and look for the method within the `Device` class. Because `Printer` inherits from `Device`
