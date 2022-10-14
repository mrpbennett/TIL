# Doing math in bash

Doing math in bash is easy when you know how. Lets see...

```bash
some_number=$((10 * 100))
```

Done! The variable `some_number` will produce 1,000. We done this by wrapping
our math in `$(())`! Cool eh! What happens if you wanted to calculate something
first well it's just like using a sum formula in Excel

```bash
math=$(((10 * 100) + 100))
```

The above calculates the `10 * 100` first before adding the 100 to it.
