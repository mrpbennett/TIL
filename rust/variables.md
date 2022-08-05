# Variables in Rust

[More information on variables](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html)

As mentioned in the “Storing Values with Variables” section, by default variables are immutable. This is one of many nudges Rust gives you to write your code in a way that takes advantage of the safety and easy concurrency that Rust offers. However, you still have the option to make your variables mutable. Let’s explore how and why Rust encourages you to favor immutability and why sometimes you might want to opt out.

When a variable is immutable, once a value is bound to a name, you can’t change that value.

### Example:

```rust
pub fn run() {
    let name = "Paul";
    let mut age = 38; // adding mut makes the variable mutable

    println!("My name is {} and is {}", name, age);

    age = 37;

    println!("No actually he's {}", age);

    const ID: i32 = 001; // must assign a data type to a const variable
    println!("ID: {}", ID)
}
```