# PYTHON > ARGS & KWARGS
## SUMMARY
> [!summary]
> `args` and `kwargs` are special syntax in Python for accepting variable-length arguments in functions:
> - `*args`: Collects extra positional arguments as a tuple
> - `**kwargs`: Collects extra keyword arguments as a dictionary
> 
> These mechanisms provide flexibility when defining functions that need to accept varying numbers of arguments.

## THEORY
### *ARGS
`*args` allows a function to accept any number of positional arguments. The `*` operator unpacks the arguments into a tuple.

```python
def sum_all(*args):
    return sum(args)

# Call with any number of arguments
result = sum_all(1, 2, 3, 4)  # 10
```

**Important**: `*args` only collects positional arguments, not key-value pairs. If you try to pass key-value pairs to `*args`, you'll get a syntax error.

### **KWARGS
`**kwargs` allows a function to accept any number of keyword arguments. The `**` operator collects these arguments into a dictionary.

```python
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

# Call with any keyword arguments
print_info(name="Alice", age=30, job="Developer")
```

**Note**: `**kwargs` is specifically designed to handle key-value pairs (keyword arguments), which `*args` cannot accept.

### COMMON MISCONCEPTIONS
- **`*args` cannot accept key-value pairs**: Only positional arguments can be collected by `*args`. For key-value pairs, always use `**kwargs`.
- **They can't be mixed up**: You can't pass keyword arguments to `*args` or positional arguments to `**kwargs`.

```python
# This works - positional args to *args, keyword args to **kwargs
def example(*args, **kwargs):
    print(args)    # Tuple of positional args
    print(kwargs)  # Dictionary of keyword args

example(1, 2, 3, name="John", age=30)  # Correct usage

# This would fail - keyword args can't go to *args
# example(*args=1)  # Syntax error
```

### CHARACTERISTICS
- Order matters: When using both, the correct order is regular arguments, `*args`, default arguments, and then `**kwargs`
- You can name them anything: While `args` and `kwargs` are conventional, any valid variable names will work
- Unpacking with `*` and `**`: You can use these operators to unpack sequences and dictionaries into function arguments

```python
def function(a, b, c):
    return a + b + c

values = [1, 2, 3]
result = function(*values)  # Unpacks list into positional arguments
```

### LIMITATIONS
- Cannot directly access specific arguments without iteration
- Type hints are challenging (use `typing.Any` or more specific TypedDict/Protocol)
- Not as self-documenting as named parameters
- Harder to enforce required parameters when using `**kwargs`

## QUESTIONS
> [!tip]- What's the difference between *args and **kwargs?
> `*args` collects excess positional arguments into a tuple, while `**kwargs` collects excess keyword arguments into a dictionary. 
> 
> ```python
> def example(*args, **kwargs):
>     print(f"args: {args}, type: {type(args)}")
>     print(f"kwargs: {kwargs}, type: {type(kwargs)}")
>
> example(1, 2, 3, name="John", age=30)
> # Output:
> # args: (1, 2, 3), type: <class 'tuple'>
> # kwargs: {'name': 'John', 'age': 30}, type: <class 'dict'>
> ```

> [!warning]- How can you define a function that accepts both required parameters and variable arguments?
> You can combine required parameters with `*args` and `**kwargs`, but the order matters:
> 1. Required positional parameters come first
> 2. Then `*args` for variable positional arguments
> 3. Then parameters with default values
> 4. Finally `**kwargs` for variable keyword arguments
>
> ```python
> def complex_function(required1, required2, *args, default1="default", default2=42, **kwargs):
>     print(f"Required: {required1}, {required2}")
>     print(f"Variable positional: {args}")
>     print(f"Defaults: {default1}, {default2}")
>     print(f"Variable keyword: {kwargs}")
>
> complex_function("must", "have", 1, 2, 3, default1="custom", extra="value")
> ```

> [!danger]- How would you implement a decorator that can handle any function regardless of its signature?
> A decorator that preserves the original function signature requires `*args` and `**kwargs` along with `functools.wraps`:
>
> ```python
> from functools import wraps
> import time
>
> def timing_decorator(func):
>     @wraps(func)  # Preserves metadata of the original function
>     def wrapper(*args, **kwargs):
>         start_time = time.time()
>         result = func(*args, **kwargs)
>         end_time = time.time()
>         print(f"Function {func.__name__} took {end_time - start_time:.6f} seconds")
>         return result
>     return wrapper
>
> @timing_decorator
> def example_function(a, b, c=3):
>     time.sleep(0.1)
>     return a + b + c
>
> # The decorator works with any function, preserving its original signature
> result = example_function(1, 2, c=5)  # Prints timing info and returns 8
> ```


- - -
#python