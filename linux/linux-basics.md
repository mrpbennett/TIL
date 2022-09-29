# Linux basics

Just a bunch of basic Linux commands

#### Linux basics

- `echo` - prints to screen
- `ls` - lists files and folders
- `cd` - change directory
- `pwd` - prints working directory
- `mkdir` - makes a directory
- `hostname` - prints the hostname

#### Directories

- `cd new-dir; mkdir www; pwd` - changes to `new-dir` creates dir `www` and
  prints working directory
- `mkdir -p /tmp/hello/world` - creates whole dir stucture the `-p` flag means:
  _“create the parent directories, too”_
- `rm -r /tmp/my-dir` - removes the directory the `-r` flag means: _to work
  recursively_
- `cp -r my-dir /tmp/my-dir` - copy directory to a new location `-r` allows
  copying of whole directory

#### Files

- `touch <filename>` - creates blank file
- `cat > test.txt` - allows you to add contents to file press `ctl-d` to
  exit...better to use vim IMO.
- `cat test.txt` - shows you contents of file
- `cp test.txt backup-test.txt` - copies the contents of file and creates a new
  one `<orignal> <copy>`
- `mv test.txt new-test.txt` - renames the file (unless specifying a dir)

#### User accounts

- `whoami` - prints our current user to the terminal
- `id` - prints out more information about current user
- `su <user>` - switch to a different user
- `ssh <user>@123.123.123.0.0` - ssh into a server as a different user

#### Download files

- `curl <some url> -O` - downloads a file using curl the `-O` flags allows the
  saving of file.
- `wget <some url> -O <file name>` - same as above but allows you to name it.

#### Check OS version

- `ls /etc/*release*` - inspects the release files
- `cat /etc/*release*` - allows more info

### Services

- `service <service> start` - will start a service
- `service <service> status` - will give the status of the service
