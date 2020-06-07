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
# settings.py
from dotenv import load_dotenv
load_dotenv()

# OR, the same with increased verbosity
load_dotenv(verbose=True)

# OR, explicitly providing path to '.env'
from pathlib import Path  # python3 only
env_path = Path('.') / '.env'
load_dotenv(dotenv_path=env_path)
```

At this point, parsed key/value from the `.env` file is now present as system environment variable and they can be conveniently accessed via 
`os.getenv()`:

```
# app.py
import os
SECRET_KEY = os.getenv("EMAIL")
DATABASE_PASSWORD = os.getenv("DATABASE_PASSWORD")
```