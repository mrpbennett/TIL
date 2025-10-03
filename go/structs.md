# Structs in Go

Structs are Go's way of creating custom data types by grouping together fields. They're the foundation of organizing data and creating object-oriented-like patterns in Go.

## Declaration and Initialization

```go
// Define a struct type
type Person struct {
    Name string
    Age  int
    Email string
}

// Initialize with field names (preferred)
p1 := Person{
    Name:  "Alice",
    Age:   30,
    Email: "alice@example.com",
}

// Initialize with positional values
p2 := Person{"Bob", 25, "bob@example.com"}

// Zero value initialization
var p3 Person  // all fields are zero values

// Partial initialization
p4 := Person{Name: "Charlie"}  // Age: 0, Email: ""
```

## Accessing and Modifying Fields

```go
person := Person{Name: "Alice", Age: 30}

// Access fields
fmt.Println(person.Name)  // "Alice"

// Modify fields
person.Age = 31
person.Email = "alice@example.com"
```

## Anonymous Structs

```go
// Useful for one-off data structures
point := struct {
    X, Y int
}{10, 20}

// Common in tests or configuration
config := struct {
    Host string
    Port int
}{
    Host: "localhost",
    Port: 8080,
}
```

## Embedded Structs (Composition)

```go
type Address struct {
    Street string
    City   string
    State  string
}

type Employee struct {
    Name    string
    Age     int
    Address Address  // nested struct
}

// Initialization
emp := Employee{
    Name: "John",
    Age:  35,
    Address: Address{
        Street: "123 Main St",
        City:   "Boston",
        State:  "MA",
    },
}

// Access nested fields
fmt.Println(emp.Address.City)  // "Boston"
```

## Struct Embedding (Promotion)

```go
type Person struct {
    Name string
    Age  int
}

type Employee struct {
    Person    // embedded - fields are promoted
    Company   string
    Salary    float64
}

emp := Employee{
    Person: Person{
        Name: "Alice",
        Age:  30,
    },
    Company: "Acme Corp",
    Salary:  75000,
}

// Can access promoted fields directly
fmt.Println(emp.Name)     // "Alice" (promoted from Person)
fmt.Println(emp.Company)  // "Acme Corp"
```

## Struct Tags

```go
type User struct {
    ID        int    `json:"id"`
    Name      string `json:"name"`
    Email     string `json:"email,omitempty"`
    Password  string `json:"-"`  // never include in JSON
    CreatedAt time.Time `json:"created_at"`
}

// Used by encoding/json, encoding/xml, and other packages
user := User{ID: 1, Name: "Alice", Email: "alice@example.com"}
jsonData, _ := json.Marshal(user)
// {"id":1,"name":"Alice","email":"alice@example.com","created_at":"..."}
```

## Methods on Structs

```go
type Rectangle struct {
    Width  float64
    Height float64
}

// Value receiver (read-only)
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Pointer receiver (can modify)
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

// Usage
rect := Rectangle{Width: 10, Height: 5}
fmt.Println(rect.Area())  // 50

rect.Scale(2)
fmt.Println(rect.Area())  // 200
```

## Use Cases

**Domain models:**
```go
type Order struct {
    ID          string
    CustomerID  string
    Items       []OrderItem
    Total       float64
    Status      string
    CreatedAt   time.Time
}

type OrderItem struct {
    ProductID string
    Quantity  int
    Price     float64
}
```

**Configuration:**
```go
type Config struct {
    ServerPort   int
    DatabaseURL  string
    MaxConnections int
    Timeout      time.Duration
    EnableLogging bool
}

func LoadConfig() (*Config, error) {
    return &Config{
        ServerPort:     8080,
        DatabaseURL:    os.Getenv("DB_URL"),
        MaxConnections: 100,
        Timeout:        30 * time.Second,
        EnableLogging:  true,
    }, nil
}
```

**API responses:**
```go
type APIResponse struct {
    Success bool        `json:"success"`
    Data    interface{} `json:"data,omitempty"`
    Error   string      `json:"error,omitempty"`
    Code    int         `json:"code"`
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
    response := APIResponse{
        Success: true,
        Data:    map[string]string{"message": "OK"},
        Code:    200,
    }
    json.NewEncoder(w).Encode(response)
}
```

**State machines:**
```go
type Connection struct {
    state      string
    retries    int
    lastError  error
    connectedAt time.Time
}

func (c *Connection) Connect() error {
    c.state = "connecting"
    // connection logic
    c.state = "connected"
    c.connectedAt = time.Now()
    return nil
}
```

**Data transfer objects:**
```go
type CreateUserRequest struct {
    Username string `json:"username" validate:"required"`
    Email    string `json:"email" validate:"required,email"`
    Password string `json:"password" validate:"required,min=8"`
}

type UserResponse struct {
    ID       int    `json:"id"`
    Username string `json:"username"`
    Email    string `json:"email"`
}
```

## Comparing Structs

```go
// Structs are comparable if all fields are comparable
type Point struct {
    X, Y int
}

p1 := Point{1, 2}
p2 := Point{1, 2}
fmt.Println(p1 == p2)  // true

// Structs with slices/maps are not comparable
type Container struct {
    Items []int  // makes struct uncomparable
}

// c1 := Container{[]int{1, 2}}
// c2 := Container{[]int{1, 2}}
// fmt.Println(c1 == c2)  // compile error
```

## Pointer vs Value

```go
// Value - creates a copy
func processValue(p Person) {
    p.Age = 50  // doesn't affect original
}

// Pointer - works with original
func processPointer(p *Person) {
    p.Age = 50  // modifies original
}

person := Person{Name: "Alice", Age: 30}
processValue(person)
fmt.Println(person.Age)  // 30

processPointer(&person)
fmt.Println(person.Age)  // 50
```

## Best Practices

**Use pointers for large structs:**
```go
// Passing by value copies entire struct
func processBig(data BigStruct) { }     // copies potentially many bytes

// Passing by pointer is efficient
func processBig(data *BigStruct) { }    // copies only pointer (8 bytes)
```

**Initialize with field names:**
```go
// Good - clear and refactor-safe
user := User{
    Name:  "Alice",
    Email: "alice@example.com",
}

// Bad - unclear and breaks when fields change
user := User{"Alice", "alice@example.com"}
```

**Use constructors for complex initialization:**
```go
func NewUser(name, email string) *User {
    return &User{
        ID:        generateID(),
        Name:      name,
        Email:     email,
        CreatedAt: time.Now(),
        Active:    true,
    }
}
```

## When to Use Structs

✅ **Use structs for:**
- Grouping related data together
- Creating custom domain types
- Implementing methods and behaviors
- Data modeling and API contracts
- Any time you need a custom composite type

Structs are fundamental to Go programming and are used extensively to create clean, maintainable code.
