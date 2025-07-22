# python > files
## Summary
> [!summary]
> File operations in Python allow reading from and writing to files using the built-in `open()` function. Operations include text or binary mode handling, reading entire files or line by line, writing content, and managing file resources with context managers.

## Theory
### Opening Files
The `open` function can handle files in text or binary mode, with different operations:
- **reading**: accessing existing file content
- **writing**: overwriting existing files or creating new ones
- **appending**: adding to existing files
- **updating**: both reading and writing

#### File Modes:
- `'r'` - read (default)
- `'w'` - write (creates new/truncates existing)
- `'a'` - append
- `'x'` - exclusive creation
- `'+'` - updating (read & write)
- `'t'` - text mode (default)
- `'b'` - binary mode

Common combinations: `'rb'`, `'r+'`, `'wb'`, `'w+b'`, `'a+'`, etc.

#### File Creation Behavior:
- `'r'` - Raises `FileNotFoundError` if file doesn't exist
- `'r+'` - Raises `FileNotFoundError` if file doesn't exist ('+' adds write capability but doesn't affect creation)
- `'w'` - Creates file if it doesn't exist, truncates if it does
- `'w+'` - Creates file if it doesn't exist, truncates if it does ('+' adds read capability)
- `'a'` - Creates file if it doesn't exist, doesn't truncate if it exists
- `'a+'` - Creates file if it doesn't exist, doesn't truncate ('+' adds read capability)
- `'x'` - Creates new file, raises `FileExistsError` if file exists
- `'x+'` - Creates new file, raises `FileExistsError` if file exists ('+' adds read capability)

```python
file = open('test.txt', mode='r')
```

### Closing Files
Always close files to free system resources:
- Using `file.close()`
- Better approach: using context manager (`with` statement)

```python
try:
    with open('newfile.txt', 'w') as file:
        file.write('new file created')
        #file.writelines(["line","anotherone"])
except FileNotFoundError as e:
    print("ERROR", e)
```

### Read Operations

```python
# Read entire file as one string
with open('file.txt', 'r') as f:
    content = f.read()  # Returns string

# Read specific number of characters
with open('file.txt', 'r') as f:
    chunk = f.read(10)  # Returns string with first 10 chars

# Read one line at a time
with open('file.txt', 'r') as f:
    line = f.readline()  # Returns string (one line)
    line2 = f.readline(5)  # Returns string with max 5 chars of next line

# Read all lines into a list
with open('file.txt', 'r') as f:
    lines = f.readlines()  # Returns list of strings

# Iterate through file (memory efficient)
with open('file.txt', 'r') as f:
    for line in f:  # File object is iterable
        print(line)
```

### Write Operations

```python
# Write string to file
with open('output.txt', 'w') as f:
    f.write('Hello World')  # Returns number of characters written
    
# Write multiple lines
with open('output.txt', 'w') as f:
    f.writelines(['Line 1\n', 'Line 2\n', 'Line 3\n'])  # No return value
    
# Write with print
with open('output.txt', 'w') as f:
    print("Hello World", file=f)  # Adds newline automatically
```

### File Object Properties
- File objects are iterators that yield lines
- Methods like `read()` return strings in text mode, bytes in binary mode
- `readlines()` returns a list of strings/bytes
- `tell()` gives current position
- `seek(offset, whence)` changes position

### Path Handling
```python
import os

# Platform independent path joining
path = os.path.join('folder', 'subfolder', 'file.txt')

# Check if file exists
if os.path.exists(path):
    # Check if it's a file (not a directory)
    if os.path.isfile(path):
        print(f"File size: {os.path.getsize(path)} bytes")
```

## Questions
> [!tip]- What's the difference between 'r+' and 'w+' modes?
> 'r+' opens for reading and writing but doesn't create or truncate the file.
> 'w+' opens for reading and writing, creates the file if it doesn't exist, and truncates it if it does.

> [!warning]- How do I handle very large files that don't fit in memory?
> Use line-by-line reading with a for loop or read in chunks:
> ```python
> # Line by line
> with open('huge_file.txt', 'r') as f:
>     for line in f:
>         process(line)
>         
> # In chunks
> with open('huge_file.txt', 'rb') as f:
>     chunk = f.read(1024)  # 1KB chunks
>     while chunk:
>         process(chunk)
>         chunk = f.read(1024)
> ```

> [!danger]- How can I safely write to files to prevent data loss?
> Use the following practices:
> 1. Always use context managers (`with` statement)
> 2. For critical data, write to temporary file and rename when complete:
> ```python
> import os
> temp_path = "data_new.txt"
> final_path = "data.txt"
> 
> with open(temp_path, 'w') as f:
>     f.write("Important data")
>     f.flush()
>     os.fsync(f.fileno())  # Ensure data is written to disk
>     
> os.replace(temp_path, final_path)  # Atomic operation
> ```

> [!tip]- How do I work with CSV files in Python?
> Use the built-in CSV module for reliable handling:
> ```python
> import csv
> 
> # Reading CSV
> with open('data.csv', 'r', newline='') as csvfile:
>     reader = csv.reader(csvfile)
>     for row in reader:
>         print(row)  # List of values
>         
> # Writing CSV
> with open('output.csv', 'w', newline='') as csvfile:
>     writer = csv.writer(csvfile)
>     writer.writerow(['Name', 'Age', 'City'])
>     writer.writerow(['Alice', '30', 'New York'])
> ```

> [!warning]- What's the proper way to handle file encodings?
> Always specify encodings explicitly to avoid platform-specific issues:
> ```python
> # UTF-8 is generally recommended
> with open('file.txt', 'r', encoding='utf-8') as f:
>     content = f.read()
>     
> # For writing
> with open('output.txt', 'w', encoding='utf-8') as f:
>     f.write('Content with non-ASCII characters: ñáéíóú')
> ```

- - -
#python