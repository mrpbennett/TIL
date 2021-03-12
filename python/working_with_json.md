# Working with JSON in python and making it pretty

I have found recently that sometimes when working with massive json responses, you're unable to see it all within your terminal window. So I like to dump it to a file. This is how I have been doing it, using `json.dumps()`

```python
focused_beats = json.dumps(sp.playlist("2gZhU46aMMpJKFIoZC5NPN"), indent=4)

with open("dump.json", "w") as f:
    f.write(focused_beats)
```

### Key Take Aways

Are...using `json.dumps()` for a json response or file, and use `json.loads()` for a json string. I have only ever worked with JSON objects / responses.

### Working with a json response / file

```python
import json

with open('Cars.json', 'r') as json_file:
    json_object = json.load(json_file)

print(json_object)

print(json.dumps(json_object))

print(json.dumps(json_object, indent=1))

""" OUTPUT """
[{'Car Name': 'Honda City', 'Car Model': 'City', 'Car Maker': 'Honda', 'Car Price': '20,000 USD'}, {'Car Name': 'Bugatti Chiron', 'Car Model': 'Chiron', 'Car Maker': 'Bugatti', 'Car Price': '3 Million USD'}]
[{"Car Name": "Honda City", "Car Model": "City", "Car Maker": "Honda", "Car Price": "20,000 USD"}, {"Car Name": "Bugatti Chiron", "Car Model": "Chiron", "Car Maker": "Bugatti", "Car Price": "3 Million USD"}]
[
 {
  "Car Name": "Honda City",
  "Car Model": "City",
  "Car Maker": "Honda",
  "Car Price": "20,000 USD"
 },
 {
  "Car Name": "Bugatti Chiron",
  "Car Model": "Chiron",
  "Car Maker": "Bugatti",
  "Car Price": "3 Million USD"
 }
]

```
It’s clear from the output that we have to pass the indent value to get the JSON data into a pretty printed format.

### Working with a python string

```python
mport json

json_data = '[{"ID":10,"Name":"Pankaj","Role":"CEO"},
              {"ID":20,"Name":"David Lee","Role":"Editor"}]'

json_object = json.loads(json_data)

json_formatted_str = json.dumps(json_object, indent=2)

print(json_formatted_str)

""" OUTPUT """
[
  {
    "ID": 10,
    "Name": "Pankaj",
    "Role": "CEO"
  },
  {
    "ID": 20,
    "Name": "David Lee",
    "Role": "Editor"
  }
]
```

using `json.loads()` to create the json object from the json string. The json.dumps() method takes the json object and returns a JSON formatted string.

[`json_dumps()` docs](https://docs.python.org/3/library/json.html#json.dumps)
