# Python Standard Library Modules

This note serves as a central reference for Python standard library modules.

## Table of Contents
- [Random](#random)
- [Time](#time)
- [Math](#math)
- [OS](#os)
- [Collections](#collections)
- [JSON](#json)
- [CSV](#csv)
- [Datetime](#datetime)

## Random

See detailed notes: [[python_random|Random Module]]

Quick reference:
```python
import random

# Core functions
random.random()       # Float between 0 and 1
random.randint(a, b)  # Integer between a and b (inclusive)
random.choice(seq)    # Random item from sequence
random.sample(seq, k) # k unique random items from sequence
random.shuffle(seq)   # Shuffle sequence in-place
```

## Time

```python
import time

# Get current time in seconds since epoch
time.time()  # 1623456789.0

# Sleep for 2 seconds
time.sleep(2)

# Format time
time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
```

## Math

```python
import math

math.sqrt(16)    # Square root: 4.0
math.floor(4.7)  # Floor: 4
math.ceil(4.1)   # Ceiling: 5
math.pi          # Pi constant: 3.141592653589793
math.e           # e constant: 2.718281828459045
```

## OS

```python
import os

# Current working directory
os.getcwd()

# List files in directory
os.listdir('path/to/dir')

# Join paths properly for the platform
os.path.join('folder', 'file.txt')

# Check if path exists
os.path.exists('path/to/check')
```

## Adding New Modules

To add a new module:
1. Add it to the Table of Contents
2. Create a new section with examples
3. For extensive notes, create a separate file like `python_modulename.md` and link to it

- - - 
#python 
