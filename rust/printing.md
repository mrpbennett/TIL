# Printing in Rust

How to print things to the console in Rust. By using the methods below.

A print function must start with `println!` if you wish to dynamically add items into your print statement you need to use the `{}` to interpolate a string. Like below in the examples

```rust
pub fn run() {
    // Print to console
    println!("Hello, from the print.rs file!");

    // Use the `{}` to interpolate a string
    println!("Hello my name is {} and I have a wife called {}", "Paul", "Fiona");

    // postional arguments
    println!("{0} is from {1} and {0} likes to {2}", "Paul", "London", "Code");

    // Named arguments
    println!("{name} is from {location}", name = "Paul", location = "London")
}
```