# Running Linux cmd link commands in bash

```bash
user=$(whomai)
dir=$(pwd)
date=$(date)

echo "You're logged in as $user in the $dir directory and today is $date"
```

In the above I ran a few simple Linux commands within my script. All that is
needed to do is.

Is wrapping the command within `$()` like above.
