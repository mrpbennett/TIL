# fmt package

[Docs](https://pkg.go.dev/fmt)

### General use case

```
%v	the value in a default format
	when printing structs, the plus flag (%+v) adds field names
%#v	a Go-syntax representation of the value
%T	a Go-syntax representation of the type of the value
%%	a literal percent sign; consumes no value
```

For example:

```go
// Printf - allows the use of template strings - %v is a variable reference
// PRintln - allows to print a line

fmt.Printf("Welcome to %v, booking application \n", conferenceName)
fmt.Printf("We have a total of %v tickets and %v are still available \n", conferenceTickets, remainingTickets)
fmt.Println("Get your tickets here to attend.")
```

You can also Scan for user input by using `fmt.Scan()` This can be used like so:

```go
var name string
fmt.Println("What's your name?")
fmt.Scan(&name)
```
