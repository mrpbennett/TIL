# Use None instead of np.nan for null values in pandas DataFrame

Sometimes a pandas DataFrame will have mixed data types. You're able to replace all null values with None (instead of default np.nan)

To do this use [`pd.DataFrame.where`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.where.html)

```python
""" Uses df value when condition is met, otherwise uses None """

df.where(df.notnull(), None)
```

||1|2|3|4|
|-|-|-|-|-|
|0|1|two|None|4|
