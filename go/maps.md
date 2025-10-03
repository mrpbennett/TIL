# Maps in Go

Maps are Go's built-in hash table implementation, providing fast key-value lookups. They're unordered collections that map keys to values, similar to dictionaries in Python or objects in JavaScript.

## Declaration and Initialization

```go
// Declare a nil map (cannot be used without initialization)
var m map[string]int

// Using make (preferred)
m := make(map[string]int)

// Map literal
m := map[string]int{
    "one":   1,
    "two":   2,
    "three": 3,
}

// Empty map literal
m := map[string]int{}

// With capacity hint (optimization)
m := make(map[string]int, 100)
```

## Basic Operations

```go
// Add or update
ages := make(map[string]int)
ages["Alice"] = 30
ages["Bob"] = 25

// Retrieve
age := ages["Alice"]  // 30

// Check if key exists (important!)
age, exists := ages["Charlie"]
if exists {
    fmt.Println("Charlie's age:", age)
} else {
    fmt.Println("Charlie not found")
}

// Delete
delete(ages, "Bob")

// Length
fmt.Println(len(ages))  // 1
```

## The "Comma Ok" Idiom

```go
m := map[string]int{"a": 1, "b": 2}

// Without checking - returns zero value if key doesn't exist
value := m["c"]  // 0 (zero value for int)

// With checking - idiomatic Go
value, ok := m["c"]
if ok {
    fmt.Println("Found:", value)
} else {
    fmt.Println("Not found")
}

// Common pattern
if value, ok := m["key"]; ok {
    // use value
}
```

## Iterating Over Maps

```go
ages := map[string]int{
    "Alice": 30,
    "Bob":   25,
    "Charlie": 35,
}

// Iterate over keys and values
for name, age := range ages {
    fmt.Printf("%s is %d years old\n", name, age)
}

// Just keys
for name := range ages {
    fmt.Println(name)
}

// Just values (use _ for keys)
for _, age := range ages {
    fmt.Println(age)
}
```

**Important:** Map iteration order is randomized intentionally!

## Use Cases

**Counting occurrences:**

```go
func countWords(text string) map[string]int {
    words := strings.Fields(text)
    counts := make(map[string]int)

    for _, word := range words {
        counts[word]++  // zero value is 0, so this works
    }

    return counts
}

text := "hello world hello"
counts := countWords(text)  // map[hello:2 world:1]
```

**Caching/Memoization:**

```go
var cache = make(map[int]int)

func fibonacci(n int) int {
    if n <= 1 {
        return n
    }

    if val, ok := cache[n]; ok {
        return val  // return cached result
    }

    result := fibonacci(n-1) + fibonacci(n-2)
    cache[n] = result
    return result
}
```

**Lookup tables:**

```go
var statusCodes = map[int]string{
    200: "OK",
    404: "Not Found",
    500: "Internal Server Error",
}

func getStatusText(code int) string {
    if text, ok := statusCodes[code]; ok {
        return text
    }
    return "Unknown"
}
```

**Grouping data:**

```go
type Person struct {
    Name string
    City string
}

func groupByCity(people []Person) map[string][]Person {
    groups := make(map[string][]Person)

    for _, person := range people {
        groups[person.City] = append(groups[person.City], person)
    }

    return groups
}
```

**Sets (using map[T]bool):**

```go
func uniqueElements(items []string) []string {
    seen := make(map[string]bool)
    unique := []string{}

    for _, item := range items {
        if !seen[item] {
            seen[item] = true
            unique = append(unique, item)
        }
    }

    return unique
}

// Alternative: map[T]struct{} uses less memory
func makeSet(items []string) map[string]struct{} {
    set := make(map[string]struct{})
    for _, item := range items {
        set[item] = struct{}{}
    }
    return set
}
```

**Configuration and options:**

```go
type ServerConfig struct {
    Port    int
    Host    string
    Options map[string]interface{}
}

config := ServerConfig{
    Port: 8080,
    Host: "localhost",
    Options: map[string]interface{}{
        "timeout":        30,
        "max_connections": 100,
        "enable_logging":  true,
    },
}
```

**Index building:**

```go
type Book struct {
    ID     int
    Title  string
    Author string
}

func buildIndex(books []Book) map[int]Book {
    index := make(map[int]Book, len(books))
    for _, book := range books {
        index[book.ID] = book
    }
    return index
}

// Fast lookup by ID
book, ok := index[42]
```

## Maps with Struct Values

```go
type User struct {
    ID       int
    Username string
    Email    string
}

users := map[int]User{
    1: {ID: 1, Username: "alice", Email: "alice@example.com"},
    2: {ID: 2, Username: "bob", Email: "bob@example.com"},
}

// Note: cannot directly modify struct fields in map
// users[1].Email = "new@example.com"  // compile error!

// Must retrieve, modify, and replace
user := users[1]
user.Email = "new@example.com"
users[1] = user

// Or use pointers
usersPtr := map[int]*User{
    1: {ID: 1, Username: "alice", Email: "alice@example.com"},
}
usersPtr[1].Email = "new@example.com"  // works!
```

## Nested Maps

```go
// Map of maps
userPreferences := make(map[string]map[string]string)

// Must initialize inner maps
userPreferences["alice"] = make(map[string]string)
userPreferences["alice"]["theme"] = "dark"
userPreferences["alice"]["language"] = "en"

// Helper function for nested access
func getPreference(userID, key string) string {
    if prefs, ok := userPreferences[userID]; ok {
        if value, ok := prefs[key]; ok {
            return value
        }
    }
    return ""
}
```

## Important Characteristics

**Maps are reference types:**

```go
m1 := map[string]int{"a": 1}
m2 := m1         // both reference same underlying map
m2["b"] = 2
fmt.Println(m1)  // map[a:1 b:2]
```

**Nil maps vs empty maps:**

```go
var nilMap map[string]int        // nil
emptyMap := map[string]int{}     // not nil
madeMap := make(map[string]int)  // not nil

fmt.Println(nilMap == nil)    // true
fmt.Println(emptyMap == nil)  // false

// Can read from nil map (returns zero value)
value := nilMap["key"]  // 0, no panic

// Cannot write to nil map
// nilMap["key"] = 1  // panic: assignment to entry in nil map
```

**Maps are not thread-safe:**

```go
// For concurrent access, use sync.Map or add your own locking
var mu sync.Mutex
var data = make(map[string]int)

func increment(key string) {
    mu.Lock()
    data[key]++
    mu.Unlock()
}
```

**Key types must be comparable:**

```go
// Valid key types: int, string, bool, pointers, structs (if all fields comparable)
map[int]string
map[string]User
map[struct{x, y int}]string

// Invalid: slices, maps, functions (not comparable)
// map[[]int]string  // compile error
```

## Performance Tips

**Pre-allocate if size is known:**

```go
// Less efficient - multiple reallocations
m := make(map[string]int)
for i := 0; i < 1000; i++ {
    m[fmt.Sprintf("key%d", i)] = i
}

// More efficient - allocate once
m := make(map[string]int, 1000)
for i := 0; i < 1000; i++ {
    m[fmt.Sprintf("key%d", i)] = i
}
```

**Use struct{} for sets to save memory:**

```go
// Uses more memory (bool = 1 byte)
set := make(map[string]bool)
set["item"] = true

// Uses less memory (struct{} = 0 bytes)
set := make(map[string]struct{})
set["item"] = struct{}{}
```

## When to Use Maps

✅ **Use maps for:**

- Fast lookups by key
- Counting/frequency analysis
- Caching and memoization
- Grouping or indexing data
- Implementing sets
- Storing key-value configuration

Maps are one of the most versatile and commonly used data structures in Go, perfect for any scenario requiring fast key-based access.
