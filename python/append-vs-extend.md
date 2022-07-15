# Append vs Extend Python lists

When adding items to a list there are two methods `append` and `extend` both have their uses

```python
list_1 = ["paul", "john", "jane"]
list_2 = ["car", "bike", "scooter"]
list_3 = []
list_4 = []

list_3.append(list_1)
list_3.append(list_2)

print("append")
print(list_3) # [['paul', 'john', 'jane'], ['car', 'bike', 'scooter']]


list_4.extend(list_1)
list_4.extend(list_2)

print("extend")
print(list_4) # ['paul', 'john', 'jane', 'car', 'bike', 'scooter']
```

As you can see from the above using append, will add lists seperatley where as extend will join each list together as one list.