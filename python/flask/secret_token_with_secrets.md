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
## BUT...this isn't best practice

It's best to keep it the same code through out your project, as you will want it to persist across restarts.

so in that case, open up your terminal

```zsh
> python
>>> import secrets
>>> print(secrets.token_hex(25)
>>> 75265f52194bdcab9dffb50d5847fcec770a8fc6344ce530cb
```
Then use `75265f52194bdcab9dffb50d5847fcec770a8fc6344ce530cb` as your secret key, like so:

```python
app.config['SECRET_KEY'] = "75265f52194bdcab9dffb50d5847fcec770a8fc6344ce530cb"
```
