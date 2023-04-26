# Check of an INT in Excel

If you're unable to see if an number in a cell is an int of a string you can use
this formular.

```
=INT(...)
```

Like so...lets say we have a number in `A1` and we're not sure if it's a string
or an int.

```
=INT(A1)=A1
```

The above will return `TRUE` if A1 is an int or `FALSE` if it's a string
