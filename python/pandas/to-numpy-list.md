# Using `to_numpy` with `tolist`

- [`to_numpy`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_numpy.html#pandas.DataFrame.to_numpy)
- [`tolist`](https://pandas.pydata.org/docs/reference/api/pandas.Series.tolist.html?highlight=tolist#pandas.Series.tolist)

```python
import pandas as pd

df = pd.DataFrame({"A": [1, 2], "B": [3, 4]})

print(df)

"""
Dataframe becomes

  A  B
0  1  3
1  2  4
"""

new_df = df.to_numpy()

print(new_df)

"""
Dataframe becomes a NumPy array after using to_numpy on it

[[1 3]
 [2 4]]
 """

print(new_df.tolist())
"""
Now we convert that NumPy array to a python list

[[1, 3], [2, 4]]
"""
```

### Note: 

By default, the dtype of the returned array will be the common NumPy dtype of all types in the DataFrame. For example, if the dtypes are `float16` and `float32`, the results dtype will be `float32`. This may require copying data and coercing values, which may be expensive.