# JAVA -> DATA TYPES -> SHORT
## SUMMARY
> [!summary]
> Short wraps the short primitive, range −32768 to 32767, providing methods like parseShort.

## THEORY
Short is less commonly used but can reduce memory in large arrays. Autoboxing and unboxing allow short to be treated like an object. Watch out for overflow when converting shorter types to int.

## QUESTIONS
> [!tip]- Why might you use Short over int?
> Possibly for memory savings in large data sets.

> [!warning]- What happens when you convert a short to int?
> The short is promoted to int, potentially changing the sign bit if not in range.

> [!danger]- Is short widely used in Java?
> Rarely, because int is generally the default integer type.
