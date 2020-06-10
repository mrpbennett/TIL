# Advance Set Operations

```python
family = {'Mum', 'Dad', 'Carly'}
aboard = {'Mum', 'Carly'}
in_transit = {'Fiona', 'Charlie'}
```

The difference() method returns a set that contains the difference between two sets.
example: 

```
<large set>.difference(<small set>)
```

```python
local_friends = family.difference(aboard)
print(local_friends)
```

The `.union()` method returns a set that contains all items from the original set, and all items from the specified sets. You can specify as many sets you want, separated by commas. If an item is present in more than one set, the result will contain only one appearance of this item.

```python
total_friends = family.union(aboard, in_transit)
print(f'Total people in all sets {total_friends}')
```

The `.intersection()` method returns a set that contains the similarity between two or more sets.

Meaning: The returned set contains only items that exist in both sets, or in all sets if the comparison is done with more than two sets.

```python
x = {"apple", "banana", "cherry"}
y = {"google", "microsoft", "apple"}

z = x.intersection(y) 

print(z)
```
