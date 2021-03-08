# Creating DataFrame using multi-dimensional list

I needed to anaylsis some data which was output as a multi-dimensional list via presto.

The dataset was out putted like this from presto

```
[['broadstreethockey.com', 104.52548999999995], ['wtop.com', 219.87889000000013], ['wcnc.com', 1421.8935000000015], ['laurenslatest.com', 130.33513000000033], ['theblackpeppercorn.com', 557.85184], ['independenttravelcats.com', 105.54693999999982]]
```

```python
# import pandas as pd 
import pandas as pd  
    
# List1  
lst = [['tom', 25], ['krish', 30], 
       ['nick', 26], ['juli', 22]] 
    
df = pd.DataFrame(lst, columns =['Name', 'Age']) 
df 
```

But now with pandas it output is like this:

```
                   Domain      Revenue
0           barnfinds.com   3514.26409
1     familyeducation.com   1011.75201
2     quirkytravelguy.com    144.26088
3           dongknows.com    352.05065
4          lifeasmama.com    225.39434
...                   ...          ...
2957      accuweather.com  32011.46441
2958     lanascooking.com    218.88943
2959     mybirthday.ninja    343.03933
2960   talent-republic.tv    209.93933
2961            cbs46.com    194.69924

[2962 rows x 2 columns]
```

Which should allow for nice data comparison