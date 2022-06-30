# Scraping table data from sites

Pretty straight forward with the [`read_html`](https://pandas.pydata.org/docs/reference/api/pandas.read_html.html?highlight=read_html) method.

```python
import pandas as pd

sandp = pd.read_html("https://en.wikipedia.org/wiki/List_of_S%26P_500_companies")

print(sandp) # will print out how many tables on the site

print(sandp[0]) # will print first table
```