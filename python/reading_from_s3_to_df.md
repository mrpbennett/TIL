# Reading from S3 convert to DataFrame

When working with Amazon S3 using `boto3`, it's important to understand that S3 doesn't have a traditional file system with folders and subfolders. Instead, it uses a flat storage structure where each item is stored as an object with a key. The key is the full path of the object within the bucket. For example, if you have a `file.csv` inside a `directory` in `my_bucket`, the key for that object would be `directory/file.txt`.

```python
""" Data Collection """

import logging
from io import StringIO

import boto3
import pandas as pd
import tomli


with open("config.toml", "rb") as f:
    c = tomli.load(f)


# S3 SESSION
s3_session = boto3.Session(
    aws_access_key_id=c["aws"]["key_id"],
    aws_secret_access_key=c["aws"]["access_key"],
)
s3_client = s3_session.client("s3")


def read_csv_from_s3(bucket_name, file_key):
    """
    Read a CSV file from S3 and return it as a pandas DataFrame
    :param bucket_name: Name of the S3 bucket
    :param file_key: Key of the file in the S3 bucket
    :return: Pandas DataFrame containing the file data
    """
    csv_obj = s3_client.get_object(Bucket=bucket_name, Key=file_key)
    body = csv_obj["Body"]
    csv_string = body.read().decode("utf-8")

    return pd.read_csv(StringIO(csv_string))
```

The above example would then be called as follows:

```python
df = read_csv_from_s3('my_bucket', 'dir/file.csv')
df.head()
```

## `io` module

The `io` module in Python is a core module that provides the main facilities for dealing with input and output operations. It is an essential part of Python's standard library and is used for working with streams (abstracted sequences of bytes like files, sockets, etc.). The module defines several classes that allow handling of various types of I/O (input/output) operations.

Key components of the `io` module include:

- **Text I/O**: These classes handle streams of text data. The most commonly used class is `io.StringIO`, which is an in-memory stream for text I/O. It allows you to work with text in memory using file-like interfaces.

- **Binary I/O**: These classes are used for handling binary data streams. The most commonly used class here is io.BytesIO, which is like `StringIO` but for bytes. It's useful for when you need a file-like interface for bytes data in memory.

- **Raw I/O**: These are low-level, base classes for raw binary I/O. They are typically used as a base for creating custom file-like objects and are not used directly in everyday coding.

- **File I/O**: The `open()` function in Python, which is commonly used to open files, is actually a shorthand for a function in the io module `(io.open())`. It returns a file object, which can be either a text or binary stream depending on the mode specified.

Here's a brief overview of the two most commonly used classes:

`io.StringIO`: This is used for in-memory text streams. It's particularly useful when you have some API that expects a file-like object, and you need to provide it with a string. Instead of writing to a file and reading back from it, you can use StringIO to simulate a file in memory.

```python
import io

# Create an in-memory text stream
stream = io.StringIO()
stream.write('Hello World!')
stream.seek(0)  # Go back to the start of the stream
print(stream.read())  # Read the contents
```

`**io.BytesIO**`: This is similar to StringIO, but it's used for handling binary data. It's useful for when you need to work with bytes, such as when dealing with binary files (like images) in memory.

```python
import io

# Create an in-memory bytes stream
byte_stream = io.BytesIO()
byte_stream.write(b'Hello World in bytes!')
byte_stream.seek(0)
print(byte_stream.read())
```

The `io` module is a versatile and powerful module for handling a wide variety of I/O operations in Python, and it is particularly useful in scenarios where you need file-like operations, but you don't necessarily want to write to or read from an actual physical file on the disk.
