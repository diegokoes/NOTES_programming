# PYTHON > VARIABLE SCOPES

## FUNCTIONS AND VARIABLES

It is essential to understand the levels of scope in Python and how things can be accessed from the four different scope levels. Below are the four scope levels and a brief explanation of where and how they are used.

### 1. LOCAL SCOPE

Local scope refers to a variable that are **declared inside a function**. For example, in the code below, the variable total is only available to the code within the get_total() function. Anything outside of this function will not have access to it.
### 2. ENCLOSING SCOPE

Enclosing scope refers to a function inside another function or what is commonly called a **nested function**.

In the code below, I added a nested function called double_it to the get_total function.

As double_it() is inside the scope of the get_total() function, it can access the variable total declared in the enclosing function. However, the local variable double, defined inside double_it(), cannot be accessed from inside the get_total() function. The function double_it() is also called the inner function.

### 3. GLOBAL SCOPE

Global scope is when a variable is declared outside of a function. This means it can be accessed from anywhere.

In the code below, I added a global variable called special. This can then be accessed from both functions get_total() and double_it():

The global variable **special** is declared outside of any function and can be accessed from any part of the program, including inside both get_total() and double_it(). The variable **total** is enclosed within get_total(), and **double** is local to double_it().

### 4. BUILT-IN SCOPE

**Built-in scope** includes **built-in functions and objects** (like print(), len(), etc.), but not **reserved keywords** (like def, for, if, etc.).
- - -
#python