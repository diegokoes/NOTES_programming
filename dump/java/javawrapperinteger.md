# Java -> Data Types -> Integer
## Summary
> [!summary]
> Integer wraps the int primitive. It includes a caching range and parse methods.

## Theory
Integer caching for values −128 to 127 prevents unnecessary object creation. Autoboxing is convenient, but large repetitive use can still harm performance. Checking Integer equality with == only works reliably within cached range.

## Questions
> [!tip]- What does Integer.valueOf() do?
> Uses the cache if the value is in the range, otherwise creates a new object.

> [!warning]- Why can == be misleading for two Integer objects?
> == compares object references, which may differ outside the cache range.

> [!danger]- How do you properly compare Integer values?
> Use .equals() for object equality.
