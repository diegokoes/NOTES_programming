# Java -> Optional
## Summary
> [!summary]-
> 
- - - 
## Theory
# Understanding Optional Methods in Java

`Optional` provides several methods to work with potentially absent values. Let's explore the key methods and their differences:

### Optional.of vs Optional.ofNullable

- **`Optional.of(value)`**: Creates an Optional containing the specified non-null value. Throws `NullPointerException` if value is null.

- **`Optional.ofNullable(value)`**: Creates an Optional containing the specified value if non-null, otherwise returns an empty Optional. This is safer when you're not certain if the value is null.

### Common Optional Methods

These are methods of the `Optional` class, not Stream methods (though some have similar names):

#### Terminal Operations (extract value or perform action)

- **`ifPresent(Consumer<T>)`**: Executes the consumer if a value is present
- **`ifPresentOrElse(Consumer<T>, Runnable)`**: Executes first action if value present, second if empty
- **`isPresent()`**: Returns true if value exists
- **`isEmpty()`**: Returns true if Optional is empty
- **`get()`**: Returns the value (throws exception if empty - generally avoided)
- **`orElse(T other)`**: Returns the value if present, otherwise returns the provided default
- **`orElseGet(Supplier<T>)`**: Returns the value if present, otherwise returns result of supplier
- **`orElseThrow()`**: Returns the value if present, otherwise throws NoSuchElementException
- **`orElseThrow(Supplier<X>)`**: Returns the value if present, otherwise throws exception from supplier

#### Intermediate Operations (returns another Optional)

- **`filter(Predicate<T>)`**: If value present and matches predicate, returns the Optional, otherwise empty
- **`map(Function<T,U>)`**: If value present, applies function to it and returns Optional of result
- **`flatMap(Function<T,Optional<U>>)`**: Similar to map but the mapping function returns an Optional

### Example Chaining

You could rewrite parts of your code with different approaches:

```java
// From your code:
Optional.ofNullable(request.getParameter("email"))
    .ifPresent(e -> {
        if (e.isBlank()) {
            mapErrores.put("email", "El email no puede estar vacío");
        } else if (!e.matches("^[\\w-\\.]+@([\\w-]+\\.)+[\\w-]{2,4}$")) {
            mapErrores.put("email", "El formato del email no es válido");
        }
    });

// Alternative approach:
Optional.ofNullable(request.getParameter("email"))
    .filter(e -> !e.isBlank())
    .filter(e -> e.matches("^[\\w-\\.]+@([\\w-]+\\.)+[\\w-]{2,4}$"))
    .ifPresentOrElse(
        email -> {}, // Valid email, do nothing
        () -> {
            String email = request.getParameter("email");
            if (email == null || email.isBlank()) {
                mapErrores.put("email", "El email no puede estar vacío");
            } else {
                mapErrores.put("email", "El formato del email no es válido");
            }
        }
    );
```

### Key Differences from Stream API

While some method names overlap with Stream API (like `filter` and `map`):

- Optional is focused on handling a single value that may be absent
- Stream is focused on processing sequences of elements

Both use functional programming concepts but serve different purposes.
## Questions
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer
- - - 
#java 