# Removing duplicate values from keys in a dict

Sometimes you need to remove duplicate values from a dictonary for example take a sellers.json file:

```json
"sellers": [
    {
      "seller_id": "539521906",
      "name": "JustPremium BV",
      "domain": "justpremium.com",
      "seller_type": "INTERMEDIARY",
      "is_passthrough": 0
    },
    {
      "seller_id": "540402547",
      "name": "JustPremium BV",
      "domain": "justpremium.com",
      "seller_type": "INTERMEDIARY",
      "is_passthrough": 0
    },
    ...
]
```

Looping through the above would give you 2 `justpremium.com` what if you wanted just one? 

You can easily remove duplicate keys by dictionary comprehension, since dictionary does not allow duplicate keys, as below:

```python
unique = { each['domain'] : each for each in sellers }.values()
```

Found the fix [here](https://stackoverflow.com/questions/33955225/remove-duplicate-json-objects-from-list-in-python/33955336)
