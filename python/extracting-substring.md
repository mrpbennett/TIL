# Extracting a substring with ReGex

I was tasked with extracting an advertiser name from a string. I came across
this
[solution](https://stackoverflow.com/questions/4666973/how-to-extract-the-substring-between-two-markers)
which seems to work very well using regex.

Using regular expressions -
[documentation](https://docs.python.org/3/library/re.html) for further reference

```python
import re

some_string = "PulsePoint_2022_PULSEPOI_COH_PHRM_DAILY_REPORT"


def get_advertiser_name(string):
    if advertiser := re.search(
        "PulsePoint_2022_PULSEPOI_(.+?)_PHRM_DAILY_REPORT", string
    ):
        return advertiser[1]


# returns COH
```
