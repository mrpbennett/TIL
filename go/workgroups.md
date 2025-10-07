# WaitGroups Explained

A **WaitGroup** is Go's way of saying "wait for a collection of goroutines to finish before continuing." Think of it as a counter that tracks how many goroutines are still running.

## The Core Concept

Imagine you're a manager who delegates tasks to employees. You need to know when everyone's done before you can leave for the day. A WaitGroup is like a checklist:

- **Add(n)**: "I'm assigning n new tasks"
- **Done()**: "One task is complete" (decrements counter)
- **Wait()**: "Block here until all tasks are done" (counter reaches zero)

## Basic Usage

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done() // Decrement counter when function returns

    fmt.Printf("Worker %d starting\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1) // Increment counter
        go worker(i, &wg)
    }

    wg.Wait() // Block until counter is 0
    fmt.Println("All workers finished")
}
```

**Output:**

```
Worker 1 starting
Worker 3 starting
Worker 2 starting
Worker 5 starting
Worker 4 starting
(1 second passes)
Worker 1 done
Worker 3 done
Worker 2 done
Worker 5 done
Worker 4 done
All workers finished
```

## The Three Methods

### 1. Add(delta int)

Adds `delta` to the WaitGroup counter. Usually called before starting a goroutine.

```go
var wg sync.WaitGroup

wg.Add(1)  // Add 1 to counter
go doWork(&wg)

wg.Add(3)  // Add 3 to counter
go task1(&wg)
go task2(&wg)
go task3(&wg)
```

**Critical:** Call `Add()` BEFORE launching the goroutine, not inside it:

```go
// WRONG - Race condition
for i := 0; i < 5; i++ {
    go func() {
        wg.Add(1) // BAD: May call Wait() before Add()
        defer wg.Done()
        doWork()
    }()
}

// CORRECT
for i := 0; i < 5; i++ {
    wg.Add(1) // GOOD: Add before launching goroutine
    go func() {
        defer wg.Done()
        doWork()
    }()
}
```

### 2. Done()

Decrements the counter by 1. Almost always used with `defer`:

```go
func worker(wg *sync.WaitGroup) {
    defer wg.Done() // Ensures Done() is called even if function panics

    // Do work...
    if someCondition {
        return // Done() still gets called
    }
    // More work...
}
```

**Why defer?** If your function has multiple return paths or might panic, `defer` guarantees `Done()` gets called.

### 3. Wait()

Blocks until the counter reaches zero:

```go
var wg sync.WaitGroup

wg.Add(10)
for i := 0; i < 10; i++ {
    go worker(i, &wg)
}

wg.Wait() // Blocks here until all 10 workers call Done()
fmt.Println("All done!")
```

## Passing WaitGroups: Pointer vs Value

**Always pass WaitGroups by pointer:**

```go
// CORRECT
func worker(wg *sync.WaitGroup) {
    defer wg.Done()
    // work...
}

var wg sync.WaitGroup
wg.Add(1)
go worker(&wg)

// WRONG - Copies the WaitGroup, Done() affects the copy, not original
func worker(wg sync.WaitGroup) {
    defer wg.Done() // This affects the COPY
    // work...
}

var wg sync.WaitGroup
wg.Add(1)
go worker(wg) // Passes a copy - Wait() never returns!
```

## Real-World Patterns

### Pattern 1: Parallel API Calls

```go
type Result struct {
    UserData    User
    OrderData   Order
    ProductData Product
}

func fetchAllData(userID int) Result {
    var wg sync.WaitGroup
    var result Result

    wg.Add(3)

    go func() {
        defer wg.Done()
        result.UserData = fetchUser(userID)
    }()

    go func() {
        defer wg.Done()
        result.OrderData = fetchOrders(userID)
    }()

    go func() {
        defer wg.Done()
        result.ProductData = fetchProducts(userID)
    }()

    wg.Wait()
    return result
}
```

### Pattern 2: Processing a Slice Concurrently

```go
func processItems(items []Item) []Result {
    var wg sync.WaitGroup
    results := make([]Result, len(items))

    for i, item := range items {
        wg.Add(1)
        go func(index int, itm Item) {
            defer wg.Done()
            results[index] = process(itm)
        }(i, item)
    }

    wg.Wait()
    return results
}
```

**Note:** Passing `i` and `item` as parameters avoids closure issues.

### Pattern 3: Worker Pool with WaitGroup

```go
func workerPool(jobs []Job) {
    var wg sync.WaitGroup
    jobsChan := make(chan Job, len(jobs))

    // Start workers
    numWorkers := 5
    for w := 0; w < numWorkers; w++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for job := range jobsChan {
                process(job)
            }
        }()
    }

    // Send jobs
    for _, job := range jobs {
        jobsChan <- job
    }
    close(jobsChan)

    wg.Wait() // Wait for all workers to finish
}
```

### Pattern 4: Nested WaitGroups

Sometimes you need to wait for groups of goroutines at different levels:

```go
func processOrders(orders []Order) {
    var outerWG sync.WaitGroup

    for _, order := range orders {
        outerWG.Add(1)

        go func(ord Order) {
            defer outerWG.Done()

            // Process each item in order concurrently
            var innerWG sync.WaitGroup
            for _, item := range ord.Items {
                innerWG.Add(1)
                go func(itm Item) {
                    defer innerWG.Done()
                    validateItem(itm)
                }(item)
            }
            innerWG.Wait() // Wait for all items in this order

            finalizeOrder(ord)
        }(order)
    }

    outerWG.Wait() // Wait for all orders
}
```

## Common Mistakes

### Mistake 1: Forgetting to Call Done()

```go
func worker(wg *sync.WaitGroup) {
    wg.Add(1) // Wrong place
    // Do work...
    // Oops, forgot wg.Done()
}

// Result: Wait() blocks forever (deadlock)
```

**Fix:** Use `defer wg.Done()` right after incrementing the counter.

### Mistake 2: Calling Add() Inside Goroutine

```go
for i := 0; i < 10; i++ {
    go func() {
        wg.Add(1) // RACE CONDITION
        defer wg.Done()
        doWork()
    }()
}
wg.Wait() // Might return before all Add() calls happen
```

**Fix:** Call `Add()` before launching goroutine.

### Mistake 3: Reusing a WaitGroup

```go
var wg sync.WaitGroup

// First batch
wg.Add(5)
for i := 0; i < 5; i++ {
    go worker(&wg)
}
wg.Wait()

// Reuse - this is OKAY
wg.Add(3)
for i := 0; i < 3; i++ {
    go worker(&wg)
}
wg.Wait()
```

This actually works fine! Once a WaitGroup reaches zero, you can reuse it. Just be careful not to call `Add()` while another goroutine might be calling `Wait()`.

### Mistake 4: Negative Counter

```go
var wg sync.WaitGroup
wg.Add(1)
wg.Done()
wg.Done() // Panic: negative WaitGroup counter
```

Calling `Done()` more times than `Add()` causes a panic.

### Mistake 5: Adding After Wait

```go
var wg sync.WaitGroup

go func() {
    time.Sleep(100 * time.Millisecond)
    wg.Add(1) // WRONG: Adding after Wait() might be called
    defer wg.Done()
    doWork()
}()

wg.Wait() // Might return before Add() is called
```

## WaitGroup vs Other Synchronization

### WaitGroup vs Channels

**WaitGroup:**

- Just tracks completion, no data transfer
- Multiple goroutines decrement same counter
- Best when you only need to "wait for completion"

**Channels:**

- Can transfer data between goroutines
- Enables communication patterns
- Best when goroutines need to coordinate or pass data

```go
// WaitGroup: Just wait
var wg sync.WaitGroup
wg.Add(3)
go task1(&wg)
go task2(&wg)
go task3(&wg)
wg.Wait()

// Channel: Wait AND collect results
results := make(chan int, 3)
go func() { results <- task1() }()
go func() { results <- task2() }()
go func() { results <- task3() }()

for i := 0; i < 3; i++ {
    result := <-results
    fmt.Println(result)
}
```

### WaitGroup vs Context

**Context** is for cancellation and timeouts, **WaitGroup** is for waiting:

```go
func withBoth(ctx context.Context) {
    var wg sync.WaitGroup

    for i := 0; i < 10; i++ {
        wg.Add(1)
        go func(id int) {
            defer wg.Done()

            select {
            case <-ctx.Done():
                return // Context cancelled
            default:
                doWork(id)
            }
        }(i)
    }

    wg.Wait() // Wait for all to finish or cancel
}
```

## Advanced Pattern: Error Collection

WaitGroups don't handle errors, but you can combine them with channels:

```go
func processWithErrors(items []Item) []error {
    var wg sync.WaitGroup
    errChan := make(chan error, len(items))

    for _, item := range items {
        wg.Add(1)
        go func(itm Item) {
            defer wg.Done()
            if err := process(itm); err != nil {
                errChan <- err
            }
        }(item)
    }

    wg.Wait()
    close(errChan)

    // Collect all errors
    var errors []error
    for err := range errChan {
        errors = append(errors, err)
    }

    return errors
}
```

## The Mental Model

Think of a WaitGroup as a **countdown latch**:

1. Set the countdown (Add)
2. Launch goroutines that decrement it when done (Done)
3. Wait for countdown to reach zero (Wait)

It's that simple! WaitGroups are your go-to tool whenever you need to "start a bunch of goroutines and wait for them all to finish."
