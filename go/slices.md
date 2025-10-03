# Slices in Go

Slices are Go's dynamic, flexible view into arrays. They're the most commonly used data structure for working with sequences, providing a powerful and convenient interface to work with collections.

## What is a Slice?

A slice is a descriptor containing:
- A pointer to an underlying array
- A length (number of elements)
- A capacity (maximum number of elements before reallocation)

## Declaration and Initialization

```go
// Declare a nil slice
var s []int  // nil, length 0, capacity 0

// Using make (preferred for pre-sizing)
s := make([]int, 5)       // length 5, capacity 5, [0 0 0 0 0]
s := make([]int, 3, 10)   // length 3, capacity 10

// Using slice literal
s := []int{1, 2, 3, 4, 5}

// From an array
arr := [5]int{1, 2, 3, 4, 5}
s := arr[1:4]  // [2 3 4]
```

## Slicing Operations

```go
s := []int{0, 1, 2, 3, 4, 5}

s[1:4]    // [1 2 3] - elements 1 through 3
s[:3]     // [0 1 2] - first 3 elements
s[2:]     // [2 3 4 5] - from index 2 to end
s[:]      // [0 1 2 3 4 5] - entire slice
```

## Appending Elements

```go
s := []int{1, 2, 3}
s = append(s, 4)           // [1 2 3 4]
s = append(s, 5, 6, 7)     // [1 2 3 4 5 6 7]

// Append another slice
other := []int{8, 9}
s = append(s, other...)    // [1 2 3 4 5 6 7 8 9]
```

## Common Operations

**Length and capacity:**
```go
s := make([]int, 3, 5)
fmt.Println(len(s))  // 3
fmt.Println(cap(s))  // 5
```

**Copying slices:**
```go
src := []int{1, 2, 3}
dst := make([]int, len(src))
copy(dst, src)  // returns number of elements copied
```

**Iterating:**
```go
for i, v := range s {
    fmt.Printf("Index: %d, Value: %d\n", i, v)
}
```

## Use Cases

**Dynamic lists:**
```go
var tasks []string
tasks = append(tasks, "Write code")
tasks = append(tasks, "Test code")
tasks = append(tasks, "Deploy code")
```

**Reading variable amounts of data:**
```go
func readLines(filename string) ([]string, error) {
    file, err := os.Open(filename)
    if err != nil {
        return nil, err
    }
    defer file.Close()
    
    var lines []string
    scanner := bufio.NewScanner(file)
    for scanner.Scan() {
        lines = append(lines, scanner.Text())
    }
    return lines, scanner.Err()
}
```

**Filtering data:**
```go
numbers := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
var evens []int

for _, n := range numbers {
    if n%2 == 0 {
        evens = append(evens, n)
    }
}
// evens: [2 4 6 8 10]
```

**Building responses:**
```go
func getActiveUsers(users []User) []User {
    active := make([]User, 0, len(users))  // pre-allocate capacity
    for _, user := range users {
        if user.Active {
            active = append(active, user)
        }
    }
    return active
}
```

**Stack operations:**
```go
// Push
stack = append(stack, item)

// Pop
item := stack[len(stack)-1]
stack = stack[:len(stack)-1]

// Peek
item := stack[len(stack)-1]
```

**Queue operations:**
```go
// Enqueue
queue = append(queue, item)

// Dequeue
item := queue[0]
queue = queue[1:]
```

## Important Behaviors

**Slices are references:**
```go
s1 := []int{1, 2, 3}
s2 := s1         // s2 points to same underlying array
s2[0] = 99       // modifies the shared array
fmt.Println(s1)  // [99 2 3]
```

**Capacity and reallocation:**
```go
s := make([]int, 0, 3)
fmt.Println(len(s), cap(s))  // 0 3

s = append(s, 1, 2, 3)
fmt.Println(len(s), cap(s))  // 3 3

s = append(s, 4)             // triggers reallocation
fmt.Println(len(s), cap(s))  // 4 6 (capacity typically doubles)
```

**Nil vs empty slices:**
```go
var nilSlice []int           // nil
emptySlice := []int{}        // not nil, but empty
makeSlice := make([]int, 0)  // not nil, but empty

fmt.Println(nilSlice == nil)    // true
fmt.Println(emptySlice == nil)  // false
fmt.Println(len(nilSlice))      // 0 (safe to use)
```

## Performance Tips

**Pre-allocate when size is known:**
```go
// Bad - causes multiple reallocations
var items []Item
for i := 0; i < 1000; i++ {
    items = append(items, Item{})
}

// Good - allocates once
items := make([]Item, 0, 1000)
for i := 0; i < 1000; i++ {
    items = append(items, Item{})
}
```

**Avoid memory leaks with large slices:**
```go
// Bad - keeps entire array in memory
func getFirst10(data []byte) []byte {
    return data[:10]  // still references full array
}

// Good - copy to new slice
func getFirst10(data []byte) []byte {
    result := make([]byte, 10)
    copy(result, data[:10])
    return result
}
```

## When to Use Slices

✅ **Use slices for:**
- Any collection that may grow or shrink
- Passing sequences to functions (efficient, no copying)
- Most list-like data structures
- Working with subsets of data

Slices are the idiomatic Go way to work with sequences and are used in almost all collection-based operations.
