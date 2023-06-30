# How to plot a Pandas Pivot table with plotly

To plot the pandas pivot table pivot_table using Plotly Express, you can follow these steps:

```python
import pandas as pd
import plotly.express as px

pivot_table = df.pivot_table(
    index="day_dt", columns="pubid", values="total_impressions", aggfunc="sum"
)

df_long = pivot_table.reset_index().melt(id_vars="day_dt", var_name="pubid", value_name="total_impressions")

fig = px.line(df_long, x="day_dt", y="total_impressions", color="pubid")

fig.show()
```

In this example, we first reset the index of `pivot_table` using the `reset_index()` method. Then, we use the `melt()` function to reshape the DataFrame to a long format, similar to what was done in the previous example.

After that, we create the line graph using `px.line`. We specify the DataFrame df_long as the input, set the x-axis values to "day_dt", the y-axis values to "total_impressions", and the color encoding to "thirdpartypubid".

Finally, we display the graph using `fig.show()`.

