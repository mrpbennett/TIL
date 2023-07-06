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


---

You can also use arguments for aliases like here:

In this case the $1 / $2 placeholders in the alias is used for command-line options, not arguments.

```bash
s3pp() {
	aws s3 $1 "s3://pp-client-onboarding/$2" --profile pp-client-onboarding
}
```