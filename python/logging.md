# Logging module

Here we can take a look at using the
[logging](https://docs.python.org/3/library/logging.html) module.

To include line numbers in your log messages, you can modify the format string
in the basicConfig method of the logging module. The format string can include
various attributes of the log record, such as the line number where the log call
was made.

```python
logging.basicConfig(level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(filename)s:%(lineno)d - %(message)s')
```

# Example usage

- `%(asctime)s` will be replaced by the time of the log record creation.
- `%(levelname)s` will be replaced by the level of the log record (e.g., INFO,
  ERROR).
- `%(filename)s` will be replaced by the filename where the log call was made.
- `%(lineno)d` will be replaced by the line number where the log call was made.
- `%(message)s` will be replaced by the log message.
-

This configuration will apply to all log messages in your script and will
include the line number in each log message, which is helpful for pinpointing
the location of events and issues in your code.

```python
import logging

logging.basicConfig(
    filename="app.log",
    filemode="w",
    format="%(name)s - %(levelname)s - %(messages)s"
)

logging.warning("This will get logged to the file")
```

- `filename="app.log"`: This is the file we are writing our logs too
- `filemode="w"`: Setting `filemode` to `w` will overwrite the default which is
  append, everytime the program runs we will get new log info
- `format`: Gives the ability to customise the log message output with this
  example would be `root - WARNING - This will get logged to the file`

`basicConfig(**kwargs)` can only be called once and is to be placed before you
start logging messages.

### Formatting the Output

You're able to format the log output by using the different
[LogRecord attributes](https://docs.python.org/3/library/logging.html#logrecord-attributes)
for example you can do the following:

```python
logging.basicConfig(format="%(process)d - %(levelname)s - %(message)s"

>>> 12345 - WARNING - This is a warning msg
```

`%(process)d`: Process ID (if available).

#### Logging Time of log

You're able to add the time of the logged message, to give your log files more
readability

```python
logging.basicConfig(
  format="%(asctime)s - %(message)s",
  datefmt="%d-%b-%y %H:%M:%S"
)
logging.warning('Admin logged out')

>>> 07-May-21 13:23:03 - Admin logged out
```

#### Logging variable data

We're able to pass variables into our logging information, for example this
could be good when a user is logged in for example

```
user = "Paul"
logging.error(f'{user} caused an error")

>>> Paul caused an error
```

### Capturing Stack Traces

Here we're able to log the actual error message

```python
import logging

a = 5
b = 0

try:
    c = a / b
except Exception as e:
    logging.error('Exception was raised', exc_info=True)
```

Using `exc_info=True` will log the error message along with what we passed. The
output would be the following:

```zsh
ERROR:root:Exception was raised
Traceback (most recent call last):
  File "/Users/paul/Developer/pycharm/playground_venv/main.py", line 7, in <module>
    c = a / b
ZeroDivisionError: division by zero
```

This way we're able to see that the reason for the crash was an
`ZeroDivisionError: division by zero`

Alternativly we could log `logging.exception()` which is the same as
`logging.error(exc_info=True)` if you wanted to log the exception on a different
rule you can do the following `logging.warning(exc_info=True)`

## Creating a custom logger

```python
import logging

logger = logging.getLogger(__name__)

c_handler = logging.StreamHandler()
f_handler = logging.FileHandler('file.log')

"""
only logs WARNING or above will be logged &
ERROR will be logged to file.
"""
c_handler.setLevel(logging.WARNING)
f_handler.setLevel(logging.ERROR)

""" this will provide the same info but the file will have a timestamp """
c_format = logging.Formatter('%(name)s - %(levelname)s - %(message)s')
f_format = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

""" assigns the formatters to the handlers """
c_handler.setFormatter(c_format)
f_handler.setFormatter(f_format)

logger.addHandler(c_handler)
logger.addHandler(f_handler)

""" test the custom logger """
logger.warning('This is an WARNING')
logger.error('This is an ERROR')
```
