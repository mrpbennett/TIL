# Renaming files

Script to rename files in multiple directories.

Add the directories you want to enter to rename the files and then use
`string_to_remove` to remove the string you want to remove from the file

```bash
#!/bin/bash

# Define an array of directories you want to process
directories=(
    "Directory_1",
    "Directory_2",
    "Directory_3",
)

# Set the string you want to remove from the filenames
string_to_remove="STRING TO CHANGE"

# Loop through each directory in the array
for directory in "${directories[@]}"; do
    # Check if the directory exists
    if [ -d "$directory" ]; then
        echo "Processing files in directory: $directory"

        # Navigate to the directory
        cd "$directory" || continue

        # Loop through each file in the directory
        for file in *; do
            # Check if the file name contains the string to remove
            if [[ $file == *"$string_to_remove"* ]]; then
                # Remove the string and rename the file
                new_name="${file//$string_to_remove/}"
                mv -i "$file" "$new_name"
                echo "Renamed: $file -> $new_name"
            fi
        done
    else
        echo "Directory does not exist: $directory"
    fi
done

```
