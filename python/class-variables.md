# Class variables

Class variables are defined within the class construction. Because they are owned by the class itself, class variables are shared by all instances of the class.

Defined outside of all the methods, class variables are, by convention, typically placed right below the class header and before the constructor method and other methods

```python
class Employee:

    # Class variables
    raise_amount = 1.04

    def __init__(self, first, last, pay):
        self.first = first
        self.last = last
        self.pay = pay
        self.email = f'{first}.{last}@company.com'

    def fullname(self):
        return f'{self.first} {self.last}'

    # Using a class variable
    def apply_raise(self):
        self.pay = int(self.pay * self.raise_amount)
```
