# Static Methods

A `@staticmethod`  takes neither a `self` nor a `cls` parameter (but of course it’s free to accept an arbitrary number of other parameters).

Therefore a static method can neither modify object state nor class state. Static methods are restricted in what data they can access - and they’re primarily a way to namespace your methods.

```Python
@staticmethod
def is_workday(day):
    if day.weekday() == 5 or day.workday() == 6:
        return False
    return True
```
