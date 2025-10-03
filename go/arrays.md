# Arrays in Go

Arrays in Go are fixed-size collections of elements of the same type. Their size is part of their type definition, making them inflexible but predictable and efficient.

## Declaration and Initialization

```go
// Declare with size
var arr [5]int  // [0 0 0 0 0]

// Declare and initialize
arr := [5]int{1, 2, 3, 4, 5}

// Let compiler count the elements
arr := [...]int{1, 2, 3, 4, 5}

// Initialize specific indices
arr := [5]int{0: 10, 2: 30, 4: 50}  // [10 0 30 0 50]
```

## Accessing Elements

```go
arr := [3]string{"Go", "Rust", "Python"}

fmt.Println(arr[0])      // "Go"
arr[1] = "C++"           // modify element
fmt.Println(len(arr))    // 3
```

## Iterating Over Arrays

```go
arr := [4]int{10, 20, 30, 40}

// Using range
for index, value := range arr {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}

// Using traditional for loop
for i := 0; i < len(arr); i++ {
    fmt.Println(arr[i])
}

// Just values
for _, value := range arr {
    fmt.Println(value)
}
```

## Multi-dimensional Arrays

```go
// 2D array
var matrix [3][3]int

// Initialize 2D array
grid := [2][3]int{
    {1, 2, 3},
    {4, 5, 6},
}

fmt.Println(grid[0][1])  // 2
```

## Use Cases

**Fixed-size data:**

```go
// Days of the week
var weekdays [7]string = [7]string{
    "Monday", "Tuesday", "Wednesday", "Thursday",
    "Friday", "Saturday", "Sunday",
}
```

**Coordinate systems:**

```go
// 3D point
point := [3]float64{10.5, 20.3, 15.7}

// RGB color
color := [3]uint8{255, 128, 0}
```

**Game boards:**

```go
// Tic-tac-toe board
var board [3][3]string

// Chess board
var chessBoard [8][8]string
```

**Buffer with known size:**

```go
var buffer [1024]byte
n, err := file.Read(buffer[:])
```

## Important Characteristics

- **Arrays are values**: When you assign or pass an array, the entire array is copied

```go
arr1 := [3]int{1, 2, 3}
arr2 := arr1  // copies all elements
arr2[0] = 10  // doesn't affect arr1
```

- **Size is part of the type**: `[3]int` and `[4]int` are different types

```go
var arr1 [3]int
var arr2 [4]int
// arr1 = arr2  // compile error: different types
```

- **Comparison**: Arrays can be compared if their element types are comparable

```go
arr1 := [3]int{1, 2, 3}
arr2 := [3]int{1, 2, 3}
fmt.Println(arr1 == arr2)  // true
```

## When to Use Arrays

✅ **Use arrays when:**

- You know the exact size at compile time
- The size will never change
- You need the performance of stack allocation
- You're working with low-level operations (like byte buffers)

❌ **Don't use arrays when:**

- You need dynamic sizing (use slices instead)
- You need to pass data efficiently (arrays are copied, slices are references)
- You're working with collections that grow or shrink

## Arrays vs Slices

In practice, **slices are used far more often than arrays** in Go. Arrays are the foundation, but slices provide a more flexible and idiomatic way to work with sequences of data.
