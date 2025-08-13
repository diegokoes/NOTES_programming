# JAVA -> DATA TYPES -> STRINGS
## SUMMARY
> [!summary]
> Strings represent sequences of characters and are immutable in Java.

## THEORY
A String is an object backed by a char array. String literals are interned for memory efficiency. Immutability simplifies concurrency and caching. You can build strings using concatenation or StringBuilder.

## QUESTIONS
> [!tip]- What is unique about Java Strings?
> They are immutable and stored in a string pool.

> [!warning]- Why is string concatenation costly in a loop?
> Each concatenation can create a new String, so use StringBuilder instead.

> [!danger]- How does the string pool work?
> Literal strings are interned, sharing references for identical text.
