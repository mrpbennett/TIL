# To prevent scientific notation in Pandas

To prevent pandas from printing long numbers in scientific notation, you can
adjust the display options using pd.options.display.float_format. Here's an
example:

```python
import pandas as pd

# Assuming you have a sorted pivot table called 'sorted_pivot_table'

# Set the display format for float values
pd.options.display.float_format = '{:.2f}'.format

# Print the sorted pivot table
print(sorted_pivot_table
```

In the code above, `'{:.2f}'.format` is used to format the float values in the
pivot table with two decimal places. You can adjust the number of decimal places
as per your requirements.

By setting `pd.options.display.float_format`, you modify the default display
format for float values in pandas. It will be applied to all subsequent print
statements until you change it again.

After setting the display format, you can use `print(sorted_pivot_table)` to
display the sorted pivot table with the modified format, where long numbers will
be displayed with the specified decimal places instead of scientific notation.

## Another method

You can also simply use the following on a DataFrame

```python
df = pd.read_csv("./data/some_data.csv", dtype={"col name": "int64"})
```

