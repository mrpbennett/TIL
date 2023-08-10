# Using read to get user input

Sometimes you need to get user input from your scripts this is where the `read`
command comes in. Let's see below:

```bash

echo "What is your name? "

read name

echo "Hello $name"
```

Here we have asked the user for their name. When they input their name. The name
will be stored in the variable `name` allowing us to reuse the name anywhere
throughout our script.
