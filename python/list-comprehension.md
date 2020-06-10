# List Comprehension

List comprehension enables python to create new lists from exsisting lists.

```python
new list = [<expr> for <elem> in <lst> if <cond>]
```

Very similar to how a for loop would work, but this is put within the new list

```python
new_list = [<variable> <for loop>]

numbers = [1, 2, 3, 4, 5, 6]
double = [n * 2 for n in numbers]
print(double)
```


This way is how you would use conditionals within list comprehensions.

```python
new_list = [<variable> <for loop> <condition>]


friends = ['Dave', 'Dale', 'Andy', 'Marytn', 'Eima']
friends_d = [d for d in friends if d.startswith('D')]
print(friends_d)
```

When using list comprehension, the new list is not the the same. A new list is created in memory.