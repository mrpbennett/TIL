# Dictionary Comprehension

Dictionary comprehension takes in a group of values, performs some kind of task on them, and returns the result

```python
users = [
    (0, "Paul", "Swordfish2@"),
    (1, "Fiona", "18ApplebyRd"),
    (2, "Dad", "Mountain1"),
    (3, "Mum", "password"),
]
```

This will loop through the list users on the 2nd item in the list. Then create a dict from that where the KEYS are usernames and the VALUES are the tuple.

```python
username_mapping = {user[1]: user for user in users}
print(username_mapping)

me = username_mapping["Paul"]
print(f"user details for Paul: {me}")
```

Alternative could be more complicated and time consuming. The below will give the same output as
`me = username_mapping["Paul"]` when used with the above dict comprehension

```python
for user in users:
    if user[1] == "Paul":
        print(user)
```

### Log in example...

`users` could be a database...

```python
users = [
    (0, "Paul", "Swordfish2@"),
    (1, "Fiona", "18ApplebyRd"),
    (2, "Dad", "Mountain1"),
    (3, "Mum", "password"),
]

username_mapping = {user[1]: user for user in users}

username_input = input('what is your username')
password_input = input('what is your password')

-, username, password = username_mapping[username_input] # (_, Paul, Swordfish2@)

if password == password_input:
    print('Login...')
else:
    print('Incorrect details...')
```
