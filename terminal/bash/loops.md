# Looping Through Bash Arrays

## Basic Array Loop

The most common way to loop through a bash array:

```bash
my_array=("apple" "banana" "cherry")

for item in "${my_array[@]}"; do
    echo "Processing: $item"
done
```

**Important:** Always use `"${my_array[@]}"` with quotes to preserve elements that contain spaces.

## Loop with Index

Access both the index and value:

```bash
my_array=("red" "green" "blue")

for i in "${!my_array[@]}"; do
    echo "Index $i: ${my_array[$i]}"
done
```

## C-Style Loop

Useful when you need traditional loop control:

```bash
my_array=("one" "two" "three")

for ((i=0; i<${#my_array[@]}; i++)); do
    echo "Element $i: ${my_array[$i]}"
done
```

## Key Syntax

- `"${array[@]}"` - Expands to all elements (quoted, preserves spaces)
- `"${!array[@]}"` - Expands to all indices
- `${#array[@]}` - Returns array length
- `${array[i]}` - Access element at index i

## Common Pitfall

```bash
# WRONG - breaks on spaces
for item in ${my_array[@]}; do
    echo "$item"
done

# CORRECT - preserves spaces
for item in "${my_array[@]}"; do
    echo "$item"
done
```

## Real-World Example

```bash
# Process multiple configuration files
config_files=(
    "config/app.yaml"
    "config/database.yaml"
    "config/cache.yaml"
)

for config in "${config_files[@]}"; do
    if [[ -f "$config" ]]; then
        echo "Loading: $config"
        # Process config file
    fi
done
```

## Creating Arrays from Commands

```bash
# Create array from command output
files=($(ls *.txt))

for file in "${files[@]}"; do
    echo "Processing: $file"
done
```

## Summary

Always remember: **Quote your array expansions** with `"${array[@]}"` to avoid word splitting issues!
