# How to find your DB users and remove them

This is how to locate what users you have created on the mySQL server.

First log into your server

```bash
mysql -uroot -p
```

### Locating your users

To list your users do the following:

```bash
mysql> SELECT user,host FROM mysql.user;
```

This will the print out all the users and their host for example:

```
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| paul             | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
```

### Deleting your users

If you wish to delete a user for what ever reason. You will need to run the following, for example lets delete user `paul`

```bash
mysql> DROP USER paul@localhost;
```

You can also delete multiple users buy just adding commas such as:

```bash
mysql> DROP USER paul@localhost, root@localhost;
```

But I wouldnt suggest deleting your `root` user.
