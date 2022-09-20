# How to show hidden files on Linux

The easiest way to show hidden files on Linux is to use the ls command with the
`-a` option for “all”.

```bash
$ ls -a <path>
```

For example here we can see all the files on my root directory

```bash
15:32:50 with ubuntu in ~ at pnfb-k3-master …
> ls -a
.   .bash_history  .bashrc  .local      .profile              .ssh                       .viminfo    .zcompdump-pnfb-k3-master-5.8      .zsh_history
..  .bash_logout   .cache   .oh-my-zsh  .shell.pre-oh-my-zsh  .sudo_as_admin_successful  .zcompdump  .zcompdump-pnfb-k3-master-5.8.zwc  .zshrc
```

Alternatively, you can use the “-A” flag in order to show hidden files on Linux.

You can then use vim to open up a file like `.zshrc`
