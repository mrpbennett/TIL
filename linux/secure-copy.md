# How to use Secure Copy SCP

Sometimes you may need to copy files from your host machine to your VM you can
do that with SCP like so:

```bash
scp <file to copy> <destination>
```

For example here we are saying we want to copy the `test.txt` file from my
Desktop and send it over to the VM that lives on `123.123.123.2` and copy the
file into the dir `/home`

```bash
scp /Users/paul/Desktop/test.txt root@123.123.123.2:/home
```
