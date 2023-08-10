# IF Statements in Bash

## IF statment

```bash
if [ condition ]; then
    # Code to execute if condition is true
fi
```

## IF ELSE statement

```bash
if [ condition ]; then
    # Code to execute if condition is true
else
    # Code to execute if condition is false
fi
```

## ELSE IF statement

```bash
if [ condition1 ]; then
    # Code to execute if condition1 is true
elif [ condition2 ]; then
    # Code to execute if condition2 is true
else
    # Code to execute if all conditions are false
fi
```

## CASE statement

```bash
case "$variable" in
    pattern1)
        # Code to execute for pattern1
        ;;
    pattern2)
        # Code to execute for pattern2
        ;;
    *)
        # Code to execute if no patterns match
        ;;
esac
```

### Test conditions:

Bash uses the test command (or [) to evaluate conditions. Here are some common
tests:

- `-eq`: Equal
- `-ne`: Not equal
- `-lt`: Less than
- `-le`: Less than or equal
- `-gt`: Greater than
- `-ge`: Greater than or equal
- `-z`: Empty string
- `-n`: Non-empty string
- `!`: Logical NOT
- `-f`: Check if file exists and is a regular file
- `-d`: Check if directory exists

### Logical operators:

You can combine conditions using logical operators:

- `&&`: Logical AND
- `||`: Logical OR
