# Getting output with `fmt`

The `fmt` package is Go's core formatting and I/O library. It's one of the first packages you'll import in nearly every Go program, providing essential functions for printing output and formatting strings.

## Core Functions

**Printing to stdout:**

```go
fmt.Print("Hello")           // prints without newline
fmt.Println("Hello")         // prints with newline
fmt.Printf("Hello %s", name) // prints with formatting
```

**String formatting:**

```go
s := fmt.Sprintf("User: %s, Age: %d", name, age)
// Creates a formatted string without printing
```

**Reading input:**

```go
var name string
fmt.Scan(&name)              // reads space-delimited input
fmt.Scanln(&name)            // reads until newline
fmt.Scanf("%s", &name)       // reads with format specifier
```

## Common Format Verbs

The real power of `fmt` comes from its format verbs:

- `%v` - default format (works with any value)
- `%+v` - includes field names for structs
- `%#v` - Go-syntax representation
- `%T` - type of the value
- `%s` - string
- `%d` - decimal integer
- `%f` - floating point
- `%t` - boolean
- `%p` - pointer address

## Practical Use Cases

**Debugging structs:**

```go
type User struct {
    Name string
    Age  int
}
user := User{"Alice", 30}

fmt.Printf("%v\n", user)   // {Alice 30}
fmt.Printf("%+v\n", user)  // {Name:Alice Age:30}
fmt.Printf("%#v\n", user)  // main.User{Name:"Alice", Age:30}
```

**Error messages:**

```go
return fmt.Errorf("failed to connect to %s: %w", host, err)
// Creates formatted error with wrapping
```

**Logging and debugging:**

```go
fmt.Printf("Processing record %d of %d\n", current, total)
fmt.Printf("Value type: %T, value: %v\n", myVar, myVar)
```

**Building dynamic strings:**

```go
query := fmt.Sprintf("SELECT * FROM users WHERE id = %d", userID)
filename := fmt.Sprintf("report_%s.pdf", date)
```

## Best Practices

- Use `fmt.Println()` for quick debugging
- Use `fmt.Printf()` when you need formatted output
- Use `fmt.Sprintf()` to build strings (more efficient than concatenation for complex formats)
- Use `%w` verb with `fmt.Errorf()` to wrap errors (Go 1.13+)
- Prefer `%v` when you're unsure of the type—it's the "universal" formatter

The `fmt` package is intentionally simple but incredibly versatile, making it the go-to tool for any output or string formatting needs in Go.
