# Salting

Salting in the context of hashing is a technique used to strengthen the security
of hashed passwords or sensitive data. Hashing, in general, involves converting
input data (like passwords) into a fixed-size string of characters using a
cryptographic hashing algorithm.

Salting introduces an extra layer of security by adding a random string of data,
known as a "salt," to the input before hashing. This salt is combined with the
original data (e.g., a password) before hashing occurs. The resulting hash is
unique to that combination of salt and input data.

**Here's an example:**

1. Hashing without salt: Let's say you have a password "password123." If
   multiple users have the same password, the hashed values will be the same.
   This can be exploited through techniques like rainbow table attacks, where
   precomputed tables of hashed values for common passwords are used to
   reverse-engineer passwords.

1. Hashing with salt: When using salt, a random string is generated for each
   password and added to it before hashing. For instance, let's add the salt
   "aBcDeF" to "password123" for one user, resulting in "password123aBcDeF".
   This combined string is then hashed. Even if another user has the same
   password, their hash will be different because their salt will be different.

**The advantages of salting include:**

1. Increased complexity: Even if two users have the same password, the hashed
   values will be different due to the unique salts, making it much harder for
   attackers to use precomputed tables like rainbow tables.

2. Protection against brute-force attacks: Salting significantly increases the
   computational effort required to crack passwords, as attackers need to
   compute hashes for each salted version of commonly used passwords.

Salting is widely used in secure password storage systems, such as in databases
or authentication systems, to enhance the security of user passwords and protect
against various types of attacks.

### Example

```python
import hashlib
import struct
import random
import string


def hash_pii(pii: str) -> str:
    hash_object = hashlib.sha512(
        (pii + "".join(random.choices(string.ascii_letters, k=30))).encode("ascii")
    ).digest()
    number = struct.unpack(">Q", b"\x00" + hash_object[:7])[0]
    return str(number)


hashed_pii = hash_pii("83.58.188.104")
```

### Example for consistent salting

This way you can unsalt data for the need to be able to run analytical queries

```python
import hashlib
import struct
import random
import string


def hash_pii(pii: str, salt:str) -> str:
    hash_object = hashlib.sha512((pii + salt.encode("ascii")).digest()
    number = struct.unpack(">Q", b"\x00" + hash_object[:7])[0]
    return str(number)


hashed_pii = hash_pii("83.58.188.104", "salty")
```
