# How to check for MacOS in bash

Sometimes you may need to check your OS if you're setting up things like
dotfiles.

For example I only want to use these scripts if I am on MacOS here is a little
neat script to check

```bash
#!/usr/bin/bash

if [[ "${uname}" == "Darwin" ]];then
    # MacOS
    . "$DOTFILES_DIR/install/zsh_install.sh"
else
    # Linux
    . "$DOTFILES_DIR/install/bash_install.sh"
fi
```

What on earth is the `uname` command??

> Uname stands for UNIX name. It is a utility to check the system information of
> your Linux computer. The uname command is commonly used to checks OS details,
> OS architecture (32 bit or 64 bit), Linux Kernel version, and Kernel release.
> When used without any options, the uname command displays the operating system
> name.

You can then run something like this in an install script:
