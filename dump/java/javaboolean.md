# JAVA -> DATA TYPES -> BOOLEAN
## SUMMARY
> [!summary]
> - Represents true or false
> - Not numerically convertible

## THEORY
boolean is used for simple flags or conditions. It's either true or false, with no numeric equivalences in standard Java.

## QUESTIONS
> [!tip]- Typical usage?
> Conditional checks, control flow, logic operations

> [!warning]- Memory usage?
> Implementation-dependent, but typically a single bit is not directly accessible

> [!danger]- Conversions?
> Java doesn't allow direct int to boolean casts. You must use explicit comparison: 
>   `boolean b = (someInt != 0);`

> [!danger]- What's the difference between boolean primitive and Boolean wrapper class? 
> The primitive can only be true/false while the wrapper is an object that can be null. Boolean wrapper provides utility methods and can be used with collections.
- - - 
#java 