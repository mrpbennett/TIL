# Using `global` to use a global var within a function

Sometimes you want to modify a global variable from within a function, you're able to use the `global` statement.

When you run the below program you will get `spam` back, as you're modifying the gloabl variable `eggs`

```python
def spam():
    global eggs
    eggs = 'spam'
        
eggs = 'global'
spam()
print(eggs)
```
