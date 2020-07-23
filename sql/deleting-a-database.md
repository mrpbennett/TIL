# Deleting a Database

Not sure how often this would be used, but If you want to delete a db from your computer locally. Maybe because you're testing something you can do it by logging into mySQL

```bash
mysql -uroot -p[your_root_password]
```

And doing the following:

```bash
mysql> DROP DATABASE database_name
```

Check if it's been deleted successfully

```bash
mysql> SHOW DATABASES;
```
