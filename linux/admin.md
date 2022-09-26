# Preventing the use of `sudo`

Own the directory you want by using chown:

```bash
sudo chown your_username directory
```

(replace your_username with your username and directory with the directory you
want.)

The other thing you can do is work as root as long as you KNOW WHAT YOU ARE
DOING. To use root do:

```
sudo -s
```

And then you can do anything without having to type sudo before every command.
