# Install mySQL via Homebrew

### Installing mySQL and setup

Now we have homebrew installed it is time to install mySQL to do that. Enter the following into the terminal and hit return.

```bash
brew install mysql
```

This will go ahead and install mySQL for you.

#### General Setup

After installing mySQL homebrew will give a few options, one of them is allowing us to set the `root` password. Or simply just log in with a blank password by:

```bash
mysql -uroot
```

I would suggest setting your root password, as it's good practice.

#### Check that mySQL is installed

Before moving on it's best to check that the install was successful. Time to start your mySQL server.

```bash
mysql.server start
```

If you see SUCCESS! you know the install has been successful. Now It's time to log in with the root user.

```
mysql -uroot
```

As the password is blank this should log us in successfully and present us with `mysql>` in the terminal.

Let's exit the server by typing in `exit` and hitting return.

#### Securing the installation

To secure your installation of mySQL you enter the following `mysql_secure_installation` in the terminal.

mySQL comes with a validation password plugin, but for this, we won't be using that. So when prompted just hit any key. Then enter your new password, this can be anything you like.

Once set you're prompted with a few options, for me personally ill remove:

-   anonymous users
-   the ability for remote login
-   the test database

Then I will reload the privileges.

#### Creating our first local database

Now we have secured our installation, it's time to create our first local database. For this database let's use the following creds:

-   user: test_user
-   password: test_password
-   database: test_database

Of course, you're able to use anything you like, but If you're not feeling creative use the above.

Let's set up our database, but first log into mySQL using the password your set above.

```bash
mysql -uroot -pyourpassword
```

Once logged in, we can create our database

```bash
mysql> CREATE DATABASE app_database;
```

Now let's create a user for the database

```bash
mysql> CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'test_password';
```

Give the new user full access to the database

```bash
mysql> GRANT ALL PRIVILEGES ON test_database.* TO 'test_user'@'localhost';
```

Now flush all privileges

```bash
mysql> FLUSH PRIVILEGES;
```

Let's exit our session by pressing `ctrl + d`

#### Log into your new DB with new user

Now everything is created let's try to log into `test_database` with user `test_user`

```bash
mysql -utest_user -ptest_password
```

If successful we know everything has worked, and I can actually write a tutorial.

Now let's try to use the `test_database` table.

```bash
mysql> USE test_database
```

If you don't get an error we're all good, and everything is set up correctly. To exit the database just type in `exit`.
