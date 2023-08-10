# Variables in bash

Using variables like any other language allows you to placeholder items for easy
use through out code so for example

```bash
#!/bin/bash

name="Paul Bennett"
now=$(date)

echo "Hello $name"
echo "System time & date is: $now"
echo "Your username is $USER"
```

We use the variable by placing a `$` in front when using it like the example
above. You can also find all the system enviroment variables by using `env` in
the terminal the output will be like so:

```bash
SHELL=/bin/bash
LSCOLORS=Gxfxcxdxdxegedabagacad
LESS=-R
HISTSIZE=
OSH=/home/ubuntu/.oh-my-bash
PWD=/home/ubuntu
LOGNAME=ubuntu
XDG_SESSION_TYPE=tty
MOTD_SHOWN=pam
HOME=/home/ubuntu
LANG=C.UTF-8
...
```
