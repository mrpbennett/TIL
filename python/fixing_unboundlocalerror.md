# Fixing UnboundLocalError

How to fix the dreaded **UnboundLocalError:** local variable referenced before assignment” after an if statement.

In the following if statement I was getting `UnboundLocalError` for the variable `matched`

```python
if form.validate():
        url = form.sellers_json_url.data

        r = requests.get(url)
        if r.status_code == 200:
            data = r.json()
            sellers = data.get("sellers")
        else:
            print(f"ERROR: {r.status_code}, {r.reason}")

        """ running data analysis on seller.json from data.py """
        try:
            connect_to_presto()
            matched = pp_sjson_comparison(data)
        except KeyError as err:
            print(err)
```

Reason for this, was because this variable would only come into play if the request was successful. To resolve this issue, I had to assign the variable before the if statement and give it a `None` value like so:

```python
matched = None # here it's assigned before it's called.

    if form.validate():
        url = form.sellers_json_url.data

        r = requests.get(url)
        if r.status_code == 200:
            data = r.json()
            sellers = data.get("sellers")
        else:
            print(f"ERROR: {r.status_code}, {r.reason}")

        """ running data analysis on seller.json from data.py """
        try:
            connect_to_presto()
            matched = pp_sjson_comparison(data)
        except KeyError as err:
            print(err)
```
