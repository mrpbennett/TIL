# Using `.env` within Python

Sweet package to allow the use of `.env` files to keep your variables in one place.

Install the latest version

```pip install -U python-dotenv```

Assuming you have created the `.env` file along-side your settings module.

```.
├── .env
└── settings.py
```

Add the following code to your `settings.py`

```python
# config.py

from os import environ, path
from dotenv import load_dotenv


basedir = path.abspath(path.dirname(__file__))
load_dotenv(path.join(basedir, '.env')) 

# OR 

load_dotenv('path-to-env')

TESTING = True
DEBUG = True
FLASK_ENV = 'development'
SECRET_KEY = environ.get('SECRET_KEY')
```

At this point, parsed key/value from the `.env` file is now present as system environment variable and they can be conveniently accessed via 
`environ.get()`:

```
# app.py
import os
import settings

SECRET_KEY = environ.get("EMAIL")
DATABASE_PWD = environ.get("DATABASE_PASSWORD")
```

```
# .env
SECRET_KEY=xxxxxx
DATABASE_PWD=xxxxxxxxxxxxxxxxxxxxx
```
