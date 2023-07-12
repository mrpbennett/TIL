# How to filter a DF

To filter data for only certain IDs in a DataFrame, you can use boolean indexing
with the `isin()` function. Here's an example:

```python
import pandas as pd

# Assuming you have a DataFrame called 'df' and you want to filter for certain IDs in the 'id' column
desired_ids = [1, 2, 3]  # List of IDs you want to include

# Filter the DataFrame for the desired IDs
filtered_df = df[df['id'].isin(desired_ids)]

# Print the filtered DataFrame
print(filtered_df)
```

In the code above, replace `'df'` with the name of your DataFrame, `'id'` with
the name of the column containing the IDs, and `[1, 2, 3]` with the list of
desired IDs.

The `df['id'].isin(desired_ids)` expression creates a boolean series where True
indicates rows where the ID value is present in the `desired_ids` list, and
False indicates rows with IDs not in the list.

By using `df[df['id'].isin(desired_ids)]`, you filter the DataFrame to only
include rows where the ID is present in the desired IDs list.

You can then use print(filtered_df) to display the filtered DataFrame containing
only the rows with the specified IDs.
