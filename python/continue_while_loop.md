# Using `continue` in a while loop

`continue` statements are used inside loops, so when a program excutes a `continue` statement. The program execution immediately jumps to the start of the loop.

```python
while True:
    name = input("Who are you?: ")
    if name != "Joe":
        continue
    print("Hello, Joe what is the password?")
    password = input()
    if password == "swordfish":
        break
print("access granted")
```

For the example above, if the user enters an name besides `Joe` the `continue` statement causes the program execution to jump back to the start of the loop.
