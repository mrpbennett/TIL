# Booleans

When it comes to comparing things to see if something is True or False you're able to use `is` or `==`

`is` is used to see if both items are the exact same.

`==` compares if two different lists in memory are the same, where as `is` wouldnt work because they're two different lists in memory and not the same list.


```python
friends = ['Dave', 'Dale']
people = ['Dave', 'Dale']

print(friends == people) # True
print(friends is people) # False
print(friends is friends) # True
'''
