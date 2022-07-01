# Classes JS & Python

This is the **basic** class layout for JS & Python

Javascript class requires a `constructor` this is the same as a `__init__` method in Python

## Javascript
```javascript
class Car {
  constructor(make, model, year) {
    this.make = make
    this.model = model
    this.year = year
  }
}

const myCar = new Car('Porsche', '911 Turbo S', '2022')

// Car { make: 'Porsche', model: '911 Turbo S', year: '2022' }
```
## Python
```python
class Car:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year

    def __repr__(self):
        return f"Make: {self.make} \nModel: {self.model} \nYear: {self.year}"


c = Car("Porsche", "911 Turbo S", "2022")
print(c)

# Make: Porsche
# Model: 911 Turbo S
# Year: 2022
```