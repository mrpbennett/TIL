# `value_count()` and it's greatness

There is a super simple way to count all your results, in a row for example using [`value_count()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.value_counts.html?highlight=value_count) - "Return a Series containing counts of unique rows in the DataFrame."

Lets take this Series:

```python
df['Hobbyist']
```

The above takes the `Hobbyist` columne from the Stackoverflow dev survey. The output is below:

```zsh
0        Yes
1         No
2        Yes
3         No
4        Yes
        ... 
88878    Yes
88879     No
88880     No
88881     No
88882    Yes
Name: Hobbyist, Length: 88883, dtype: object
```

This is now a series and it has 88,882 entries, couting those could require a loop or a function, with pandas it's super simple.

```python
df['Hobbyist'].value_counts()
```

```zsh
Yes    71257
No     17626
Name: Hobbyist, dtype: int64
```

Easy pease!
