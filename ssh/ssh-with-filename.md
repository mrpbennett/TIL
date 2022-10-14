# Creating a SSH key with a filename

Sometimes you may need to create more than one SSH key. This is the way to
create one with a file name

```bash
ssh-keygen -f <filename> -t ed25519
```

So for example the following would be like the below, if we were creating an SSH
for github

```bash
ssh-keygen -f ~/.ssh/github-key -t ed25519
```

You could even create an alias like so:

```bash
alias skg="ssh-keygen -f ~/.ssh/$1 -t ed25519
```

The above will allow you to just type `skg <filename>`
