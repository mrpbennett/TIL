# How to disable pylint warnings

Open up your `pylintrc` file. Mine was blank at the time

```bash
~/.pylintrc
```

You can then add the following:

```bash
[MESSAGE CONTROL]
disable=invalid-name
disable=line-too-long
```

And save, this has now disabled `invalid-name` which highlights names which are not snake case. Although I write all my variables and functions in snake case. Things like `db` are used when using databases.

`lint-too-long` these are generally comments, as Black takes care of everything else.