# Creating a pivot table

[`pandas.DataFrame.pivot_table`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot_table.html?highlight=pivot_table#pandas.DataFrame.pivot_table)

```python
import pandas as pd

# import data set from csv

df = pd.read_csv('<some file>')

# select the cols you wish to use for the model
df = df [['col1', 'col2', 'col3']]

# create a pivot table from those cols
pivot = df.pivot_table(index='col1', columns='col2', values='col3', aggfunc='sum')

# export to excel
pivot.to_excel('<some file>', sheet_name='sheet1', startrow=4)
```