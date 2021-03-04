# How to generate a SECRET_KEY with secrets

If a secret key is set, cryptographic components can use this to sign cookies and other things. Set this to a complex random value when you want to use the secure cookie for instance.

```python
from flask import Flask
import secrets

# generate a secret key via secrets
secret_key = secrets.token_hex(16)

app = Flask(__name__)
app.config["SECRET_KEY"] = secret_key
...
```

The secrets module allows you to create a `token_hex()` instead of generating one from the terminal you can let secrets handle that for you. By simply importing secrets and adding the following:

```python
secret_key = secrets.token_hex(16)
```

Then assigning it to your `app.config` like so:

```python
app.config["SECRET_KEY"] = secret_key
```
