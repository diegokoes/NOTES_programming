# JAVA -> DATA TYPES
1. Primitive Tyes
	- **[[javabyte|byte]]**
	- **[[javashort|short]]**
	- **[[javaint|int]]**
	- **[[javalong|long]]**
	- **[[javafloat|float]]**
	- **[[javadouble|double]]**
	- **[[javachar|char]]**
	- **[[javaboolean|boolean]]**
2. Reference Types
	- **[[javaclasses|Classes]]**
	- **[[javainterfaces|Interfaces]]**
	- **[[javaarrays|Arrays]]**
	- **[[javastrings|Strings]]**
	- **[[javaenums|Enums]]** 
	- **[[javarecords|Records]]**
3. Wrapper Types
	- **[[javawrapperbyte|Byte]]**
	- **[[javawrappershort|Short]]**
	- **[[javawrapperinteger|Integer]]**
	- **[[javawrapperlong|Long]]**
	- **[[javawrapperfloat|Float]]**
	- **[[javawrapperdouble|Double]]**
	- **[[javawrappercharacter|Character]]**
	- **[[javawrapperboolean|Boolean]]**

## SUMMARY
> [!summary]
> 
- - - 
## THEORY

Autoboxing and unboxing are mechanisms in Java that handle automatic conversions between primitive data types and their corresponding wrapper classes. These features were introduced in Java 5 (also known as Java 1.5) to make code more readable and reduce the verbosity that was previously required when working with primitives and objects.

### AUTOBOXING

Autoboxing is the automatic conversion of a primitive type to its corresponding wrapper class object. For example, converting an `int` to an `Integer` or a `boolean` to a `Boolean`. Before Java 5, you would have had to explicitly create these wrapper objects:

```java
// Before Java 5
Integer boxedInt = new Integer(42);

// With autoboxing (Java 5 and later)
Integer boxedInt = 42; // Java automatically converts int to Integer
```

When you assign a primitive value to a variable of its wrapper class type, Java silently creates the appropriate wrapper object for you. This happens in many contexts:

```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(10); // Autoboxing: int 10 is converted to an Integer object

Map<Character, Integer> charCount = new HashMap<>();
charCount.put('A', 1); // Autoboxing: char 'A' to Character, int 1 to Integer
```

### UNBOXING

Unboxing is the reverse process – the automatic conversion of a wrapper class object to its corresponding primitive type. For example, converting an `Integer` to an `int` or a `Boolean` to a `boolean`:

```java
// Before Java 5
int primitiveInt = boxedInt.intValue();

// With unboxing (Java 5 and later)
int primitiveInt = boxedInt; // Java automatically extracts the primitive value
```

Like autoboxing, unboxing happens automatically in various contexts:

```java
Integer boxedInt = new Integer(42);
int result = boxedInt + 10; // Unboxing: boxedInt is converted to primitive before addition

Boolean isValid = Boolean.TRUE;
if (isValid) { // Unboxing: Boolean object is converted to boolean primitive
    System.out.println("Valid");
}
```

## IMPORTANT CONSIDERATIONS

While autoboxing and unboxing make code cleaner, there are several important aspects to understand:

1. **Performance Implications**: These conversions create objects and can impact performance, especially in tight loops or memory-constrained environments. Each autoboxing operation allocates a new object on the heap.

2. **NullPointerException Risk**: Unboxing a null wrapper object throws a NullPointerException:

   ```java
   Integer nullInteger = null;
   int value = nullInteger; // Throws NullPointerException when unboxing
   ```

3. **Caching for Common Values**: For certain ranges of values, wrapper classes maintain a cache of pre-created objects. For example, `Integer` typically caches values from -128 to 127. This means:

   ```java
   Integer a = 100;
   Integer b = 100;
   System.out.println(a == b); // true, because both reference the same cached object
   
   Integer c = 1000;
   Integer d = 1000;
   System.out.println(c == d); // false, outside cache range, so separate objects
   ```

4. **Type Hierarchy**: While primitive types have no inheritance relationship, their wrapper classes do. For instance, `Integer` extends `Number`, enabling polymorphism.

Understanding autoboxing and unboxing helps you write cleaner code while being aware of potential pitfalls. These features are particularly useful when working with Java's collection frameworks (which can only store objects, not primitives) and with generic types.

## QUESTIONS
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer

- - - 
#java