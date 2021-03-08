# Accessing JSON in Pandas

I wanted to put some json from a url into a pandas dataframe this was the JSON

```json
{
    "contact_email": "demand-support@adyoulike.com",
    "sellers": [
        {
            "domain": "vuukle.com",
            "is_confidential": 0,
            "name": "Vuukle [HB]",
            "seller_id": "002223867dc2abc8f61d382472da7690",
            "seller_type": "INTERMEDIARY"
        },
        {
            "domain": "oyada.xyz",
            "is_confidential": 0,
            "name": "TELECARE",
            "seller_id": "0039bf03278f83a8c29e862fa6a7d6cf",
            "seller_type": "PUBLISHER"
        },
        {
            "domain": "zemanta.com",
            "is_confidential": 0,
            "name": "EXT - Zemanta",
            "seller_id": "00588d1868c60c12d50c15f14bd1b2a6",
            "seller_type": "PUBLISHER"
        }
        ...
}
```

This is how I put the JSON data into it's own data frame, then drilled down to get what I needed which was the domain value.

```python
sjon_data = data
df = pd.json_normalize(sjon_data, record_path=["sellers"], errors="ignore")
df_domains = df.domains
```

`sjson_data` stored the data I pulled via requests, this was similar to what is above.

Then I used [`.json_normalized`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.json_normalize.html) to allow me to use `record_path`

##### record_pathstr or list of str, default None
Path in each object to list of records. If not passed, data will be assumed to be an array of records.

Which printed out the following dataframe: 

```
      seller_id                         name            domain   seller_type  is_passthrough
0     540605781                 Revolvy, LLC       revolvy.com     PUBLISHER               0
1     539582155    Interworks Media Co. Ltd.     iwmedia.co.kr  INTERMEDIARY               0
2     541213556               JustPremium BV   justpremium.com  INTERMEDIARY               0
3     540870601              Rough Maps Inc.     roughmaps.com     PUBLISHER               0
4     539521962               JustPremium BV   justpremium.com  INTERMEDIARY               0
...         ...                          ...               ...           ...             ...
6456  537145258        The Movie Insider LLC  movieinsider.com     PUBLISHER               0
6457  537144646                 Vonetize PLC       takoomi.com  INTERMEDIARY               0
6458  537153161              Spil Games B.V.     spilgames.com     PUBLISHER               0
6459  539297087  CMG Clique Media Group Inc.        cmginc.com     PUBLISHER               0
6460  539296078    Spine Media, LLC (via EB)        google.com  INTERMEDIARY               0
```

To then access only the domains I just had to use `df.domains` which gave me the following:

```
0            revolvy.com
1          iwmedia.co.kr
2        justpremium.com
3          roughmaps.com
4        justpremium.com
              ...
6456    movieinsider.com
6457         takoomi.com
6458       spilgames.com
6459          cmginc.com
6460          google.com
```