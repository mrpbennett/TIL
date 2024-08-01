# Check for tabs in txt files

At work we have thousands of data sets in txt files. Generally some look like so:

```txt
source prefix encryptionMethod secretKey salt
domain1.com RQ123456 SHA2-256
domain2.com RQ654836 SHA2-256
domain3.com RQ098098 SHA2-256
domain4.com RQ010101 SHA2-256
```

And when it comes to making a commit sometimes it's easy to leave off a tab in emtpy cols. To check this you can run a `cat` command like so:

```bash
cat -te filename.txt
```

Which will print out tabs as `^I` therefore if you don't see that symbol where a tab should be it's not there.
