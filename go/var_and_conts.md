# Variables and Constants

Variables and constants are ways of storing data. Variables are mutable
whereas constants are immutable.

```go
// variables can be declared like so:

name := "Paul"
var name string = "Paul"
```

The above shows that a variable can be declared with an explicit type
(`string`, `int`, `float32`, etc.), or a variable can be declared via `:=`
where the compiler infers the type from the assigned value.

Constants on the other hand cannot be changed and are immutable.

```go
const iCantBeChanged = "someString"
```

Here the constant `iCantBeChanged` can only ever have the string
"someString" assigned to it.

You can also assign multiple variables to a function for example

```go
result, err := someFunction()
if err != nil {
    // handle the error
    return err
}
// use result
```

This is great because:

- **Explicit error handling**: Unlike exceptions in other languages, Go forces you to explicitly check for errors at each step
- **No hidden control flow**: You can see exactly where errors might occur
- **Clear contracts**: Functions that can fail return an error as their last return value
