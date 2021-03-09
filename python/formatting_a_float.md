# How to format a float

When returning a float sometimes you want it to format as a revenue number.

```python
revenue = round(revenue, 2)
# 308716.98
```

But what if I wanted `308,716`

We can use `revenue:,.0f` the special bit is:

```python
:,.0f
```

 - `f` states that revenue is a float number, and you want to control its format.
 - `.0` states that you want 0 positions after `.`
 - `,` says that you want `,` for thousands
 - `:-` is a seperator between your variable name and formatting controllers
