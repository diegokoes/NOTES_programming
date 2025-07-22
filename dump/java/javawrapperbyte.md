# Java -> Data Types -> Byte
## Summary
> [!summary]
> Byte wraps the byte primitive, providing utility methods and a range of −128 to 127.

## Theory
Byte is often used for streams or memory-constrained operations. Autoboxing converts byte to Byte automatically:

byte b = 10;
Byte boxed = b; // Autoboxed

## Questions
> [!tip]- What is the default value of a Byte object?
> null, because Byte is a reference type.

> [!warning]- Why be cautious with autoboxing in loops?
> Each autoboxing can create a new object, impacting performance.

> [!danger]- What happens if you unbox a null Byte?
> A NullPointerException is thrown.
