# Java -> Data Types -> Long
## Summary
> [!summary]
> Long wraps the long primitive for larger integer values, also with caching for small values.

## Theory
Long covers a wider range than int, commonly used for timestamps. Autoboxing from long to Long simplifies collections usage, but performance overhead is higher than using the primitive.

## Questions
> [!tip]- When should you choose Long over int?
> When you need values beyond int’s range or when storing nulls is required.

> [!warning]- How does autoboxing impact memory with Long?
> Each autobox can create a new object, increasing garbage collection.

> [!danger]- How do you compare two Long objects safely?
> Use Long.compare or .equals().
