# Using GroupBy

Let's say we have a CSV file contains columns for `NPI`, `LIST_ID`, and `SF_Deliverable_ID`. To split out each `LIST_ID` with its own NPIs, you can use the following Python script with pandas:

```python
import pandas as pd

# Load the CSV file
file_path = 'FTD_NPI_Summary.csv'
data = pd.read_csv(file_path)

# Group by LIST_ID and create separate DataFrames for each
list_id_groups = data.groupby('LIST_ID')

# Save each group to a separate CSV file
for list_id, group in list_id_groups:
    group.to_csv(f'LIST_ID_{list_id}.csv', index=False)

print("CSV files created for each LIST_ID.")
```

To send the response to an endpoint using a `PUT` request, you can use the requests library in Python. Below is a script that reads the grouped data and sends a `PUT` request to the specified endpoint for each `LIST_ID`.

```python
# Load the CSV file
file_path = 'FTD_NPI_Summary.csv'
data = pd.read_csv(file_path)

# Group by LIST_ID
list_id_groups = data.groupby('LIST_ID')

# Base URL for the API endpoint
base_url = 'https://lifeapi.pulsepoint.com/RestApi/v1/npi/npi-list/'

# Iterate through each LIST_ID group and send a PUT request
for list_id, group in list_id_groups:
    npis = group['NPI'].tolist()
    payload = {
        "npis": npis
    }
    url = f'{base_url}{list_id}'

    response = requests.put(url, json=payload)

    if response.status_code == 200:
        print(f'Successfully updated LIST_ID {list_id}')
```

Used - [pandas.DataFrame.groupby](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.groupby.html#pandas-dataframe-groupby)
