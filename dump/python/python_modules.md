# PYTHON -> MODULES

This note serves as a central reference for Python standard library modules.
modules come from modular programming (functionality broken into blokcs -> scope reusability, simplicity).

different modules can have functions of the same name. 

why does sinmplicity help avoid inter dependency?

modules are searched current directory-> built in module dir -> pythonpath -> installation dependent default directory 

pip!

import sys 
sys.path.insert(1,r'path')
import x...?

import math as m

from math import factorial,sqrt,log10

python library built in modules:
## TABLE OF CONTENTS
- [Random](#random)
- [Time](#time)
- [Math](#math)
- [OS](#os)
- [Collections](#collections)
- [JSON](#json)
- [CSV](#csv)
- [Datetime](#datetime)

## RANDOM

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

## TIME

```python
import time

# Get current time in seconds since epoch
time.time()  # 1623456789.0

# Sleep for 2 seconds
time.sleep(2)

# Format time
time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
```

## MATH

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

## ADDING NEW MODULES

To add a new module:
1. Add it to the Table of Contents
2. Create a new section with examples
3. For extensive notes, create a separate file like `python_modulename.md` and link to it

reload() -> what for  dynamic changes


# MODULE USE-CASES

So far you have learned about modules, packages and libraries in the context of using modules in Python. The third-party packages in Python are in most cases open-source, free and available for a wide variety of domains. These resources extend the functionality of Python programs beyond the built-in modules and are one of the main reasons why Python is popular today.

Before you learn how to install and use these packages, let's briefly go through the difference between a module, a package and a library.

## MODULES, LIBRARIES AND PACKAGES

Modules and packages can easily be confused because of their similarities, but there are a few differences. Modules are similar to files, while packages are like directories that contain different files. Modules are generally written in a single file, but that's more of a practice than a definition.

Packages are essentially a type of module. Any module that contains the __path__ definition is a package. Packages, when viewed as a directory, can contain sub-packages and other modules. Modules, on the other hand, can contain classes, functions and data members just like any other Python file.

Library is a term that's used interchangeably with imported packages. But in general practice, it refers to a collection of packages.

Despite the differences between modules, packages and libraries, you can import any of them using import statements.

Third-party package add-ons of Python can be found in the Python Package Index. To install packages that aren't a part of the standard library programmers use ‘pip’ (package installer for Python). It is installed with Python by default. To use pip, you need to be familiar with either the terminal if you're using a Mac or the command line interface if you're using Windows.

Alternatively, you can also use the terminal window present inside your IDE. When you are using the command line or terminal, you must make sure that you are installing packages in the same Python interpreter that you are working with inside your IDE.

(I use cachyos (arch linux) and vscode, leave the info for windows and cachyos) I use pyenv too.

## SUB-PACKAGES

If we are to assume that packages are similar to a folder or directory in our operating system, then the package can also contain other directories. Packages, both built-in and user-defined, can contain other folders within them that need to be accessed. These are named sub-packages. You use dot-notation to access sub-packages within a package you have imported. For example, in a package such as matplotlib, the contents that are used most commonly are present inside the subpackage pyplot. Pyplot eventually may consist of various functions and attributes.

The code for importing a sub-package is:

1

To make it even more convenient, it is often imported using an alias. So more commonly, you will come across code such as:

1

You could use any other word as an alias instead of plt, but it is a common convention.

You can explore the directory structure of such packages usually by searching for the module index of that package.