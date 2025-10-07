# Sequential Programming vs Goroutines

Let me break down the fundamental differences, when to use each, and how they compare in real scenarios.

## Sequential Programming

**Sequential execution** means code runs one operation at a time, line by line, in order:

```go
func sequential() {
    result1 := fetchUser(1)      // Wait for completion
    result2 := fetchUser(2)      // Then wait for this
    result3 := fetchUser(3)      // Then wait for this

    process(result1, result2, result3)
}
```

If each `fetchUser` takes 1 second, this takes **3 seconds total**.

## Concurrent with Goroutines

```go
func concurrent() {
    var wg sync.WaitGroup
    results := make([]User, 3)

    for i := 0; i < 3; i++ {
        wg.Add(1)
        go func(id int) {
            defer wg.Done()
            results[id] = fetchUser(id)
        }(i)
    }

    wg.Wait()
    process(results[0], results[1], results[2])
}
```

All three requests happen simultaneously. Total time: **~1 second** (the time of the slowest request).

## The Mental Models

### Sequential: A Single Chef

Imagine a chef making three pizzas:

1. Make dough for pizza 1 → wait for it to rise → bake → done
2. Make dough for pizza 2 → wait for it to rise → bake → done
3. Make dough for pizza 3 → wait for it to rise → bake → done

**Total time: 3 × (prep + rise + bake)**

### Concurrent: Multiple Chefs (Goroutines)

Three chefs work in parallel:

- Chef 1 handles pizza 1
- Chef 2 handles pizza 2
- Chef 3 handles pizza 3

All pizzas rise and bake simultaneously.

**Total time: 1 × (prep + rise + bake)**

## Real-World Comparison

### Example 1: API Calls to Multiple Services

**Sequential approach:**

```go
func getOrderDetails(orderID string) OrderDetails {
    user := callUserService(orderID)           // 100ms
    product := callProductService(orderID)     // 150ms
    payment := callPaymentService(orderID)     // 120ms
    shipping := callShippingService(orderID)   // 90ms

    return OrderDetails{user, product, payment, shipping}
}
// Total time: 100 + 150 + 120 + 90 = 460ms
```

**Concurrent approach:**

```go
func getOrderDetails(orderID string) OrderDetails {
    var user User
    var product Product
    var payment Payment
    var shipping Shipping

    var wg sync.WaitGroup
    wg.Add(4)

    go func() {
        defer wg.Done()
        user = callUserService(orderID)
    }()

    go func() {
        defer wg.Done()
        product = callProductService(orderID)
    }()

    go func() {
        defer wg.Done()
        payment = callPaymentService(orderID)
    }()

    go func() {
        defer wg.Done()
        shipping = callShippingService(orderID)
    }()

    wg.Wait()
    return OrderDetails{user, product, payment, shipping}
}
// Total time: max(100, 150, 120, 90) = 150ms
```

**Speedup: 3x faster!**

### Example 2: Processing Large Dataset

**Sequential:**

```go
func processImages(images []Image) []ProcessedImage {
    results := make([]ProcessedImage, len(images))

    for i, img := range images {
        results[i] = resize(img)  // Takes 50ms per image
    }

    return results
}
// 1000 images = 50 seconds
```

**Concurrent with Worker Pool:**

```go
func processImages(images []Image) []ProcessedImage {
    numWorkers := runtime.NumCPU() // e.g., 8 cores
    jobs := make(chan Image, len(images))
    results := make(chan ProcessedImage, len(images))

    // Start workers
    for w := 0; w < numWorkers; w++ {
        go func() {
            for img := range jobs {
                results <- resize(img)
            }
        }()
    }

    // Send jobs
    for _, img := range images {
        jobs <- img
    }
    close(jobs)

    // Collect results
    processed := make([]ProcessedImage, 0, len(images))
    for i := 0; i < len(images); i++ {
        processed = append(processed, <-results)
    }

    return processed
}
// 1000 images on 8 cores ≈ 6-7 seconds (8x faster)
```

## When Sequential is BETTER

Not everything should be concurrent. Here are cases where sequential is preferable:

### 1. **Dependent Operations**

When each step depends on the previous result:

```go
func processOrder(order Order) error {
    // These MUST happen in order
    validated := validateOrder(order)
    if !validated {
        return errors.New("invalid order")
    }

    charged := chargePayment(order)
    if !charged {
        return errors.New("payment failed")
    }

    shipped := shipOrder(order)
    return shipped
}
```

Making this concurrent would be pointless and potentially incorrect.

### 2. **Simple, Fast Operations**

```go
// Sequential is fine - total time is negligible
func calculateTotal(items []Item) float64 {
    total := 0.0
    for _, item := range items {
        total += item.Price * item.Quantity
    }
    return total
}
```

The overhead of spawning goroutines would exceed any benefit.

### 3. **Small Datasets**

```go
// Not worth making concurrent
func processSmallBatch(items []string) {
    for _, item := range items {
        process(item)
    }
}
```

If you have 5 items, sequential is simpler and equally fast.

### 4. **Strict Ordering Requirements**

```go
// Must maintain order
func generateReport(data []DataPoint) string {
    report := ""
    for _, point := range data {
        report += formatDataPoint(point) // Order matters
    }
    return report
}
```

## When Goroutines SHINE

### 1. **I/O-Bound Operations**

Anything involving waiting (network, disk, databases):

```go
// Perfect for goroutines
func aggregateDataFromAPIs() {
    var wg sync.WaitGroup

    wg.Add(3)
    go func() {
        defer wg.Done()
        fetchFromAPI1() // Waiting on network
    }()
    go func() {
        defer wg.Done()
        fetchFromAPI2() // Waiting on network
    }()
    go func() {
        defer wg.Done()
        fetchFromAPI3() // Waiting on network
    }()

    wg.Wait()
}
```

### 2. **CPU-Intensive Parallel Work**

When tasks can truly run in parallel:

```go
// Good for goroutines (with worker pool)
func calculatePrimeFactors(numbers []int) {
    numCPU := runtime.NumCPU()
    // Distribute work across CPU cores
    // Each core works independently
}
```

### 3. **Event-Driven Systems**

Handling multiple concurrent events:

```go
func handleWebSocketConnections() {
    for {
        conn := acceptConnection()
        go handleClient(conn) // Each client gets own goroutine
    }
}
```

### 4. **Background Tasks**

```go
func startServer() {
    go periodicCleanup()      // Runs in background
    go metricsCollector()     // Runs in background
    go healthCheckMonitor()   // Runs in background

    startMainServer()         // Main work
}
```

## The Tradeoffs

### Sequential Advantages

- **Simple**: Easy to reason about, debug, and test
- **Predictable**: No race conditions, no synchronization bugs
- **Low overhead**: No goroutine creation cost
- **Ordered**: Natural execution order maintained

### Sequential Disadvantages

- **Slow for I/O**: Waits unnecessarily during I/O operations
- **Underutilizes resources**: CPU cores sit idle
- **Poor responsiveness**: Blocks on long operations

### Goroutine Advantages

- **Fast for I/O**: Doesn't wait idly during I/O
- **Utilizes resources**: Can use all CPU cores
- **Responsive**: Can handle multiple things at once
- **Scalable**: Can handle thousands of concurrent tasks

### Goroutine Disadvantages

- **Complex**: Harder to reason about, debug, and test
- **Race conditions**: Shared memory requires careful synchronization
- **Overhead**: Small cost per goroutine (though tiny compared to threads)
- **Can be overkill**: For simple, fast operations

## Decision Framework

Ask yourself:

**1. Is there waiting involved (I/O)?**

- Yes → **Goroutines** likely help
- No → **Sequential** might be fine

**2. Are operations independent?**

- Yes → **Goroutines** can parallelize
- No → **Sequential** is necessary

**3. Is the dataset large enough?**

- Yes (100s-1000s+ items) → **Goroutines** with worker pool
- No (< 100 items) → **Sequential** is simpler

**4. Am I already fast enough?**

- Yes → **Sequential** (simpler is better)
- No → Profile first, then consider **Goroutines**

## Hybrid Approach (Often Best)

You don't have to choose one exclusively:

```go
func processOrderBatch(orders []Order) {
    // Concurrent fetching (I/O-bound)
    var wg sync.WaitGroup
    enrichedOrders := make([]EnrichedOrder, len(orders))

    for i, order := range orders {
        wg.Add(1)
        go func(idx int, ord Order) {
            defer wg.Done()
            enrichedOrders[idx] = enrichOrder(ord) // Parallel I/O
        }(i, order)
    }
    wg.Wait()

    // Sequential processing (order matters)
    for _, enriched := range enrichedOrders {
        saveToDatabase(enriched) // Must be sequential for consistency
    }
}
```

## The Bottom Line

**Start sequential, add concurrency where it helps.** Premature optimization with goroutines adds complexity without guaranteed benefits. Profile your code, identify bottlenecks, then strategically add goroutines where I/O waiting or CPU parallelization makes sense.

Go's philosophy: Make concurrency easy, but don't force it everywhere. Use it when it actually solves a problem.
