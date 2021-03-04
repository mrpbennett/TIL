# How to compare two files


This solution reads both files in one pass, excludes blank lines, and prints common lines regardless of their position in the file:

```python
with open('some_file_1.txt', 'r') as file1:
    with open('some_file_2.txt', 'r') as file2:
        same = set(file1).intersection(file2)

same.discard('\n')

with open('some_output_file.txt', 'w') as file_out:
    for line in same:
        file_out.write(line)
```

Link to solution [here](https://stackoverflow.com/questions/19007383/compare-two-different-files-line-by-line-in-python)
