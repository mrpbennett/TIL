# Deep Dive into Goroutines

## What Are Goroutines Really?

Goroutines are **not** OS threads. They're more like "green threads" or coroutines - user-space threads managed entirely by the Go runtime. This distinction is crucial:

- **OS Thread**: Typically 1-2 MB stack size, expensive context switching, managed by the kernel
- **Goroutine**: Starts with ~2 KB stack (grows/shrinks dynamically), cheap context switching, managed by Go runtime

The Go runtime multiplexes thousands of goroutines onto a small number of OS threads using a scheduler.

## The Go Scheduler (M:N Scheduling)

Go uses an **M:N scheduler** where M goroutines run on N OS threads. The key components:

- **G** (Goroutine): The actual goroutine with its stack and state
- **M** (Machine): An OS thread
- **P** (Processor): A scheduling context (usually one per CPU core)

The runtime maintains a pool of threads and intelligently schedules goroutines onto them. If a goroutine blocks (waiting for I/O, channel operation, etc.), the scheduler moves other goroutines onto available threads so CPU cores stay busy.

## Starting and Managing Goroutines

### Basic Syntax

```go
// Anonymous function
go func() {
    fmt.Println("Running in goroutine")
}()

// Named function
go myFunction(arg1, arg2)

// Method call
go obj.Method()
```

### Important: Goroutine Lifecycle

```go
func main() {
    go fmt.Println("Hello from goroutine")
    // main() exits immediately - goroutine never runs!
}
```

The program exits when `main()` returns, killing all goroutines. You need synchronization:

```go
func main() {
    go fmt.Println("Hello from goroutine")
    time.Sleep(time.Second) // Bad practice, but illustrates the point
}
```

## Goroutine Communication Patterns

### 1. WaitGroups (Synchronization)

The proper way to wait for goroutines to finish:

```go
var wg sync.WaitGroup

for i := 0; i < 10; i++ {
    wg.Add(1)  // Increment counter
    go func(id int) {
        defer wg.Done()  // Decrement when done
        doWork(id)
    }(i)
}

wg.Wait()  // Block until counter reaches 0
```

**Critical mistake** many beginners make:

```go
for i := 0; i < 10; i++ {
    go func() {
        fmt.Println(i)  // All print 10! (closure captures variable)
    }()
}

// Correct version: pass as parameter
for i := 0; i < 10; i++ {
    go func(id int) {
        fmt.Println(id)  // Prints 0-9
    }(i)
}
```

### 2. Context (Cancellation and Timeouts)

The idiomatic way to cancel goroutines:

```go
func worker(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            fmt.Println("Cancelled!")
            return
        default:
            doWork()
        }
    }
}

ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

go worker(ctx)
go worker(ctx)

// After 5 seconds or when cancel() is called, all workers stop
```

## Real-World Patterns

### 1. Worker Pool (Controlled Concurrency)

Don't spawn unlimited goroutines - control concurrency:

```go
func processJobs(jobs []Job, numWorkers int) {
    jobsChan := make(chan Job, len(jobs))
    results := make(chan Result, len(jobs))

    // Start fixed number of workers
    for i := 0; i < numWorkers; i++ {
        go worker(jobsChan, results)
    }

    // Send jobs
    for _, job := range jobs {
        jobsChan <- job
    }
    close(jobsChan)

    // Collect results
    for i := 0; i < len(jobs); i++ {
        result := <-results
        handleResult(result)
    }
}

func worker(jobs <-chan Job, results chan<- Result) {
    for job := range jobs {
        result := process(job)
        results <- result
    }
}
```

### 2. Rate Limiting

Control how fast goroutines execute:

```go
func rateLimitedWorker() {
    limiter := time.Tick(100 * time.Millisecond)  // 10 requests/second

    for task := range tasks {
        <-limiter  // Wait for rate limiter
        go processTask(task)
    }
}

// Or with token bucket
func tokenBucketRateLimiter() {
    bucket := make(chan struct{}, 10)  // 10 tokens

    // Refill bucket
    go func() {
        ticker := time.NewTicker(time.Second)
        for range ticker.C {
            select {
            case bucket <- struct{}{}:
            default:
            }
        }
    }()

    // Consume tokens
    for task := range tasks {
        <-bucket  // Wait for token
        go processTask(task)
    }
}
```

### 3. Fan-Out, Fan-In Pattern

Distribute work, then collect results:

```go
func fanOut(input <-chan int) []<-chan int {
    numWorkers := 5
    channels := make([]<-chan int, numWorkers)

    for i := 0; i < numWorkers; i++ {
        ch := make(chan int)
        channels[i] = ch

        go func(out chan<- int) {
            for n := range input {
                out <- n * n  // Process data
            }
            close(out)
        }(ch)
    }

    return channels
}

func fanIn(channels []<-chan int) <-chan int {
    out := make(chan int)
    var wg sync.WaitGroup

    for _, ch := range channels {
        wg.Add(1)
        go func(c <-chan int) {
            defer wg.Done()
            for n := range c {
                out <- n
            }
        }(ch)
    }

    go func() {
        wg.Wait()
        close(out)
    }()

    return out
}
```

## Goroutine Pitfalls and Best Practices

### 1. Goroutine Leaks

Goroutines that never exit waste memory:

```go
// BAD: Goroutine leaks if channel never receives
func leak() {
    ch := make(chan int)
    go func() {
        val := <-ch  // Blocks forever if nothing sends
        fmt.Println(val)
    }()
    // Oops, forgot to send to ch or close it
}

// GOOD: Use context for cancellation
func noLeak(ctx context.Context) {
    ch := make(chan int)
    go func() {
        select {
        case val := <-ch:
            fmt.Println(val)
        case <-ctx.Done():
            return
        }
    }()
}
```

### 2. Shared Memory Races

```go
// BAD: Data race
counter := 0
for i := 0; i < 1000; i++ {
    go func() {
        counter++  // Multiple goroutines writing = race condition
    }()
}

// GOOD: Use mutex or atomic operations
var mu sync.Mutex
counter := 0
for i := 0; i < 1000; i++ {
    go func() {
        mu.Lock()
        counter++
        mu.Unlock()
    }()
}

// BETTER: Use atomic
var counter int64
for i := 0; i < 1000; i++ {
    go func() {
        atomic.AddInt64(&counter, 1)
    }()
}
```

### 3. Closing Channels

Only the sender should close a channel, never the receiver:

```go
// GOOD
func producer(out chan<- int) {
    for i := 0; i < 10; i++ {
        out <- i
    }
    close(out)  // Producer closes
}

// BAD: Multiple senders can't safely close
```

## Performance Characteristics

**When to use goroutines:**

- I/O-bound tasks (network requests, file operations)
- Independent parallel computations
- Event handling and background tasks

**When NOT to use:**

- CPU-bound tasks where you'd spawn more goroutines than CPU cores (use worker pools)
- Very short-lived operations (overhead of goroutine creation might exceed benefits)
- When order matters and you'd need complex synchronization anyway

**Benchmarking goroutines:**

```go
func BenchmarkGoroutines(b *testing.B) {
    for i := 0; i < b.N; i++ {
        var wg sync.WaitGroup
        wg.Add(1)
        go func() {
            defer wg.Done()
            // Do work
        }()
        wg.Wait()
    }
}
```

The beauty of goroutines is they make concurrent programming accessible, but you still need to understand synchronization, avoid races, prevent leaks, and control concurrency appropriately. They're a powerful tool when used correctly.
