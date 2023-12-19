# Access the directory and retrieve files

Now we are going to write a function that will access a certain path in the
folder, loop through the files and get the latest one using its metadata. I have
added some additional protections and bits of code which you can understand from
the inline comments.

Parameters:

- `sftp`: The connection object we obtained in the previous code.
- `date_limit`: The number of days back `int` we would ask the function to scan
  the files in the SFTP folder.
- `remote_path`: The path `str` in which we want to search files, within the
  SFTP folder. This method will return the file which we ranked as the latest
  `f`, and another boolean value called to_load.

`to_load` will only become True once there is at least one file that meets all
of our limitations. It means the file is within our date limit, and is not empty
(or larger than a decided size). You can use to_load for further
implementations, i.e: continue your code only if a file was actually retrieved.

```python
import paramiko
from datetime import datetime, timedelta
import traceback

def get_files_from_sftp(sftp, date_limit, remote_path):
    """ get new files from sftp and load to s3 """
    to_load = False  # will set to True if there are files to load (for later implementation)
    max_last_modified = datetime.now() - timedelta(days=7)  # set a date variable to compare files to

    files = sftp.listdir(remote_path)  # get files in directory
    # check for new files above date limit
    max_file_path = ""
    try:
        for f in files:
            # get last modified date/timestamp from file metadata
            last_modified = sftp.stat(remote_path + f).st_mtime
            last_modified_ts = datetime.fromtimestamp(last_modified)
            last_modified_date = datetime.fromtimestamp(last_modified).date()

            if last_modified_date > date_limit:  # check limit
                if sftp.stat(remote_path + f).st_size > 1000:  # check if file is empty (in this case larger than 1MB)
                    if last_modified_ts > max_last_modified:  # maintain last modified file
                        max_last_modified = last_modified_ts
                        max_file = f
                        to_load = True

        print(max_file)
        return to_load, max_file

    except:
        trace_error = traceback.format_exc()
        print('something is wrong - did not get files \n' + trace_error)
```

You can now use the `max_file` object in any way you want. You can download it
to your local machine, or keep it in memory and perform further actions.
