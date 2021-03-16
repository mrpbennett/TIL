# DataFrames vs Series.

What are the differences? 

## Dataframe

DataFrame is a 2-dimensional labeled data structure with columns of potentially different types. You can think of it like a spreadsheet or SQL table.

```zsh
       Respondent                                         MainBranch Hobbyist  \
0               1             I am a student who is learning to code      Yes   
1               2             I am a student who is learning to code       No   
2               3  I am not primarily a developer, but I write co...      Yes   
3               4                     I am a developer by profession       No   
4               5                     I am a developer by profession      Yes   
...           ...                                                ...      ...   
88878       88377                                                NaN      Yes   
88879       88601                                                NaN       No   
88880       88802                                                NaN       No   
88881       88816                                                NaN       No   
88882       88863                                                NaN      Yes   
```

## Series

Pandas Series is a one-dimensional labeled array capable of holding data of any type (integer, string, float, python objects, etc.). The axis labels are collectively called index. Pandas Series is nothing but a column in an excel sheet.

```zsh
0                United Kingdom
1        Bosnia and Herzegovina
2                      Thailand
3                 United States
4                       Ukraine
                  ...          
88878                    Canada
88879                       NaN
88880                       NaN
88881                       NaN
88882                     Spain
Name: Country, Length: 88883, dtype: object
```
