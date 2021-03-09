# Merging two Dataframes and removing duplicates

Merging two dataframes is pretty easy to do using pandas when you can used [`DataFrame.merge()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html?highlight=merge#pandas-dataframe-merge)

In the example below I used that here:

```python
df_merged = df_rpt.merge(df_sj, how="inner", on="domain", indicator=True)
```
When merging two data frames there are sometimes duplicates. I removed these using [`DataFrame.drop_duplicates`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html#pandas.DataFrame.drop_duplicates)

Here:

```python
df_merged_no_dupes = df_merged.drop_duplicates(subset=["domain"]).reset_index(drop=True)
```

Full example of merging two data frames then removing duplicates.

```python
 df_merged = df_rpt.merge(df_sj, how="inner", on="domain", indicator=True)
    df_merged_no_dupes = df_merged.drop_duplicates(subset=["domain"]).reset_index(
        drop=True
    )
```
