# PYTHON > RANDOM MODULE

The `random` module provides functions for generating random numbers and making random selections.

## COMMON FUNCTIONS

```python
import random

# Generate random float between 0 and 1
random.random()  # Returns 0.7012297463869929 (example)

# Random integer between a and b (inclusive)
random.randint(1, 10)  # Returns a random integer between 1 and 10

# Random float between a and b
random.uniform(1.5, 10.5)  # Returns a random float between 1.5 and 10.5

# Random choice from a sequence
fruits = ['apple', 'banana', 'cherry', 'orange']
random.choice(fruits)  # Returns one random item from the list

# Random sample from a sequence
random.sample(fruits, 2)  # Returns a list of 2 unique random items

# Shuffle a list in-place
random.shuffle(fruits)  # Shuffles the original list
```

## SEEDING

For reproducible results, you can set a seed:

```python
random.seed(42)  # All subsequent random operations will be deterministic
random.random()  # Always returns 0.6394267984578837 when seed is 42
```

## CHOICE FUNCTION

```python
# random.choice() selects a random element from a non-empty sequence
colors = ['red', 'green', 'blue', 'yellow']
random.choice(colors)  # Might return 'blue'

# Works with any sequence type
random.choice('abcdefg')  # Might return 'c'
random.choice(tuple(range(10)))  # Might return 7
```

## RANDOM SAMPLING

```python
# Sample without replacement (each item can only appear once)
deck = ['ace', 'king', 'queen', 'jack', '10', '9']
hand = random.sample(deck, 3)  # Might return ['king', '9', 'ace']

# Sample with replacement
hand = [random.choice(deck) for _ in range(3)]  # Could have duplicates
```

- - -

