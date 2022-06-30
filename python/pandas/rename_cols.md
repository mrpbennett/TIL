# Renaming Cols with Pandas

[`pandas.DataFrame.rename`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rename.html?highlight=rename#pandas.DataFrame.rename)

```python
df = pd.read_csv('some-dir-to-csv')

df.rename(columns={'<old col name>':'<new col name>'}, inplace=True)
```

`inplace=True` - Whether to return a new DataFrame. If True then value of copy is ignored.