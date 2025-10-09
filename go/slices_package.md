# Slices package...

The slices package defines various functions useful with slices of any type. Below are some small examples I came across today.

I wanted to see how I could remove a string from a slice. Below I have a slice called `names`

```go
names := []string{"Paul", "Fiona", "Murphy", "Linda"}
```

I wanted to remove the string `Linda`. In the block below I used two functions from the slice package these were [`Index`](https://pkg.go.dev/slices#Index) and [`Delete`](https://pkg.go.dev/slices#Index) these two allowed me to get the index of the string and delete it by its index.

```go
if idx := slices.Index(names, "Linda"); idx != -1 {
    slices.Delete(names, idx, idx+1)
}
```

So what is happening here?

`slices.Index`: takes two arguments, the slice in question `names` and the item I am searching for.

> Index returns the index of the first occurrence of v in s, or -1 if not present.

In the `if` statement you can see a `;` this is used to combine two conditions. Allowing for the statement to read as follows. IF the index of "Linda" does not equal `-1` then action the code inside the block. Pretty cool as this is different from Python which I am use to.

Next there is `slices.Delete` this takes three arguments, the slice, the start index and the end index. This would work in the same manner as a slice would work in Python `[1:4]` for example. Therefore with the block above, I took the index of the string, for this example lets say it was 3 and I only wanted to delete that index, therefore the index would be 4.

This is why the arguments are `Delete(names, idx, idx+1)` which in turn would be `Delete(names, 3, 3+1)`

Quite simple but was am ah ha moment for me.

More can be found on the slices package [here](https://pkg.go.dev/slices#pkg-overview)
