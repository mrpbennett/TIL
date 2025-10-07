# Goroutines and Channels

## Goroutines

A **goroutine** is Go's lightweight thread managed by the Go runtime. They're incredibly cheap - you can spawn thousands or even millions of them without issues.

To start a goroutine, you just prefix a function call with the `go` keyword:

```go
go myFunction()
```

This launches the function in a separate goroutine while your main program continues executing.

## Channels

**Channels** are the pipes that connect goroutines, allowing them to communicate and synchronize. They're typed conduits through which you can send and receive values.

Creating a channel:

```go
ch := make(chan int)  // unbuffered channel
ch := make(chan int, 10)  // buffered channel with capacity 10
```

Sending and receiving:

```go
ch <- 42      // send value to channel
value := <-ch // receive value from channel
```

## Real World Examples

### 1. **Web Scraper**

Imagine you need to scrape 100 websites. Instead of doing them sequentially (which would take forever), you spawn a goroutine for each:

```go
func scrapeWebsite(url string, results chan<- string) {
    // Fetch and parse the website
    content := fetchURL(url)
    results <- content  // Send result back through channel
}

urls := []string{"site1.com", "site2.com", ...}
results := make(chan string, len(urls))

for _, url := range urls {
    go scrapeWebsite(url, results)
}

// Collect all results
for i := 0; i < len(urls); i++ {
    content := <-results
    fmt.Println(content)
}
```

### 2. **Worker Pool for Image Processing**

A common pattern is having a pool of workers processing jobs from a queue:

```go
func worker(id int, jobs <-chan Image, results chan<- ProcessedImage) {
    for img := range jobs {
        processed := resizeAndCompress(img)
        results <- processed
    }
}

// Create job and result channels
jobs := make(chan Image, 100)
results := make(chan ProcessedImage, 100)

// Start 5 worker goroutines
for i := 1; i <= 5; i++ {
    go worker(i, jobs, results)
}

// Send jobs
for _, img := range images {
    jobs <- img
}
close(jobs)
```

### 3. **Real-time Data Aggregator**

Think of a stock trading system that needs to aggregate price updates from multiple exchanges:

```go
func watchExchange(exchange string, prices chan<- PriceUpdate) {
    for {
        update := getLatestPrice(exchange)
        prices <- update
    }
}

prices := make(chan PriceUpdate)

go watchExchange("NYSE", prices)
go watchExchange("NASDAQ", prices)
go watchExchange("LSE", prices)

// Aggregate all price updates
for update := range prices {
    processUpdate(update)
}
```

### 4. **HTTP Server with Timeout**

Channels can implement timeouts elegantly:

```go
func fetchWithTimeout(url string, timeout time.Duration) (string, error) {
    result := make(chan string, 1)

    go func() {
        data := slowAPICall(url)
        result <- data
    }()

    select {
    case data := <-result:
        return data, nil
    case <-time.After(timeout):
        return "", errors.New("timeout")
    }
}
```

## Key Patterns

**Fan-out, Fan-in**: Multiple goroutines read from the same channel (fan-out) or multiple goroutines send to a channel that one goroutine reads (fan-in).

**Pipeline**: Chain goroutines together where the output of one is the input to the next, like: generate numbers → square them → filter odds → print.

The beauty of goroutines and channels is how they make concurrent programming feel natural. Instead of dealing with threads, mutexes, and locks, you "communicate by sharing memory" through channels.
