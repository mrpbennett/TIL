# Creating random numbers randomly

I have recently been working on a project that requires some numbers that have a length of 10 they also have to be unqiue. I was a pain to manually type them out in excel so here we go

```python
from random import randint

def random_with_N_digits(n):
    range_start = 10**(n-1)
    range_end = (10**n)-1
    return randint(range_start, range_end)
    

for _ in range(10):
    print(random_with_N_digits(10))
```
