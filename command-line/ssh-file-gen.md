# Creating two new ssh files

```zsh
[ -e ~/.ssh/id_rsa.pub ] || yes '' |ssh-keygen -N '' -t rsa
cat ~/.ssh/id_rsa.pub
```

This will create 2 new files `id_rsa` and `id_rsa.pub`
