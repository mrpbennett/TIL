# Altering a users password

Sometimes we just want to change our password. We can simply do this by doing the following

```bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```

This will change the `'root'@'localhost'` account password. Replace the password with the password that you want to use. To change the password for a root account with a different host name part, modify the instructions to use that host name.
