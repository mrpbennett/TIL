# Using arguments in bash

If you have ever ran a script like so:

```bash
somescript.sh something
```

You know what arguments are, well positional arguments anyways. This is how
they're used...

```bash
#somescript.sh

name=$1
title=$2

echo "Hello $name you're a $title
```

You would then run the script like so:

```bash
somescript.sh Paul SeniorSolutionsEngineer
```

To use multiple just increase the number after the `$`
