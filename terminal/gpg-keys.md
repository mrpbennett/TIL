# Creating and using GPG Keys

### Encrypting a file

The `-c` flag in GPG uses symmetric encryption (password-based), not public key encryption. If you want to encrypt with a specific key, you'll need to use `-e` (encrypt) instead.
Here are the different approaches:

For public key encryption with a specific key:

```bash
gpg -e -r recipient@email.com file.txt
```

or

```bash
gpg -e -r KEY_ID file.txt
```

**If you want symmetric encryption but with key-derived passwords:**
You'd still use `-c`, but GPG will prompt for a passphrase:

```bash
gpg -c file.txt
```

To see available keys:

```bash
gpg --list-keys          # public keys
gpg --list-secret-keys   # your private keys
```

### Deleting a key

To delete GPG keys associated with an email address:

1. First, list the keys to see what you have:

```bash
gpg --list-keys email@domain.com
gpg --list-secret-keys email@domain.com
```

2. Delete the secret key first (if it exists):

```bash
gpg --delete-secret-keys email@domain.com
```

3. Then delete the public key:

```bash
gpg --delete-keys email@domain.com
```

**Batch deletion (use with caution):**

```bash
gpg --batch --yes --delete-secret-keys email@domain.com
gpg --batch --yes --delete-keys email@domain.com
```
