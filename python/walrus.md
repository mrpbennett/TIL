# The walrus operator

In Python, the `:=` operator is known as the "walrus operator." It was
introduced in Python 3.8 as part of the PEP 572 proposal. The walrus operator
allows you to assign a value to a variable as part of an expression. It's
particularly useful in situations where you want to both assign a value to a
variable and use that value within an expression.

```python
# Without walrus operator
while True:
    user_input = input("Enter a number (or 'exit' to quit): ")
    if user_input == 'exit':
        break
    number = int(user_input)
    if number > 10:
        print("The number is greater than 10")

# With walrus operator
while True:
    if (user_input := input("Enter a number (or 'exit' to quit): ")) == 'exit':
        break
    number = int(user_input)
    if number > 10:
        print("The number is greater than 10")
```

In this example, the walrus operator `:=` is used to both assign the value of
`input("Enter a number (or 'exit' to quit): ")` to the user_input variable and
check if it equals 'exit' within the same if statement.

The walrus operator can help make code more concise and readable, especially in
scenarios where you want to avoid repeating the same expression multiple times.
However, it should be used judiciously to maintain code clarity and avoid
overcomplicating logic.
