# Using `.sample` with random

When trying to create a list of random numbers I always went for `randint(1, 10)` as example. This would generate a number between 1 & 10, however this would create duplicate numbers.

**Hello [`.sample()`](https://docs.python.org/3/library/random.html#random.sample)**

This will return a list of 10 numbers selected from the range 0 to 99, without duplicates.

```python
import random
random.sample(range(100), 10)
```