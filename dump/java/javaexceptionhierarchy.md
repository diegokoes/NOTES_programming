# JAVA -> EXCEPTIONS ->  HIERARCHY
## SUMMARY
> [!summary]
> Java's exception mechanism is built around a class hierarchy with Throwable as the root class. This hierarchy branches into Error (serious problems) and Exception (recoverable conditions). Exceptions are further divided into checked exceptions (must be explicitly handled) and unchecked exceptions (RuntimeException subclasses), each serving different purposes in error handling. Understanding this hierarchy helps developers choose the appropriate exception types for different error scenarios.

## EXCEPTION CLASS HIERARCHY

```
                              java.lang.Throwable
                                    ↙       ↘
                          Error               Exception
                          ↙                      ↙     ↘
       VirtualMachineError                   IOException, SQLException...  RuntimeException
       OutOfMemoryError                       (Checked Exceptions)           ↙     ↓      ↘
       StackOverflowError                                       NullPointerException  IllegalArgumentException
       ...                                                      ArithmeticException    IndexOutOfBoundsException
                                                               ...                    ...
                                                               (Unchecked Exceptions)
```

### ROOT: THROWABLE

`java.lang.Throwable` is the root class of the exception hierarchy. All objects that can be thrown and caught in Java are instances of Throwable or one of its subclasses. Key methods:

- `getMessage()`: Returns detailed error message
- `getStackTrace()`: Returns stack trace elements
- `printStackTrace()`: Prints stack trace to standard error stream
- `getCause()`: Returns the cause of this exception

### PRIMARY BRANCHES

#### 1. ERROR

`java.lang.Error` represents serious problems that a reasonable application should not try to catch. Most Error instances are abnormal conditions that should never occur.

**Key Error subclasses:**

- `VirtualMachineError`: Problems with the JVM
  - `OutOfMemoryError`: JVM ran out of memory
  - `StackOverflowError`: Stack space overflowed
- `LinkageError`: Class dependencies changed incompatibly
  - `NoClassDefFoundError`: Required class not found
- `AssertionError`: Assertion failed

#### 2. EXCEPTION

`java.lang.Exception` is the superclass of all checked exceptions. This branch represents conditions that a reasonable application might want to catch.

### CHECKED VS. UNCHECKED EXCEPTIONS

#### CHECKED EXCEPTIONS

Checked exceptions extend `Exception` but not `RuntimeException`. They **must** be either:
1. Caught in a try-catch block, or
2. Declared in the method signature with a throws clause

The compiler enforces this requirement. Checked exceptions represent expected but exceptional conditions that well-written applications should anticipate and recover from.

**Common checked exceptions:**

- `IOException`: Input/output operations failures
  - `FileNotFoundException`: Attempt to access a nonexistent file
  - `EOFException`: End of file or stream reached unexpectedly
- `SQLException`: Database access errors
- `ClassNotFoundException`: Class not found during reflection
- `InterruptedException`: Thread interrupted
- `ParseException`: Error during parsing

#### UNCHECKED EXCEPTIONS

Unchecked exceptions fall into two categories:
1. Subclasses of `RuntimeException`
2. Subclasses of `Error`

These do not need to be caught or declared. They typically represent programming errors or catastrophic conditions.

**Common RuntimeException subclasses:**

- `NullPointerException`: Attempt to use null where an object is required
- `IllegalArgumentException`: Method received an illegal argument
  - `NumberFormatException`: Failed to convert string to number
- `IllegalStateException`: Object in inappropriate state for operation
- `IndexOutOfBoundsException`: Index outside valid range
  - `ArrayIndexOutOfBoundsException`: Array index out of bounds
  - `StringIndexOutOfBoundsException`: String index out of bounds
- `UnsupportedOperationException`: Operation not supported
- `ConcurrentModificationException`: Concurrent modification of collection
- `ClassCastException`: Invalid cast attempt
- `ArithmeticException`: Arithmetic error (e.g., division by zero)

## DESIGN PHILOSOPHY

### WHY TWO TYPES OF EXCEPTIONS?

- **Checked Exceptions**: For recoverable conditions where the API designer expects callers to handle the exceptional condition
- **Unchecked Exceptions**: For programming errors and unrecoverable situations

### EXCEPTION PROPAGATION

1. When an exception occurs, the JVM creates an exception object
2. The exception propagates up the call stack until:
   - It's caught by an appropriate catch block
   - It reaches the top of the thread's stack (causing program termination)

### CHOOSING EXCEPTION TYPES

Guidelines for deciding which exception type to use:

| Use Case | Recommended Exception Type |
|----------|----------------------------|
| Recoverable application condition | Checked Exception |
| Programming error | RuntimeException subclass |
| Resource exhaustion | RuntimeException subclass |
| Precondition violation | IllegalArgumentException or similar |
| Impossible state | Error subclass (rare for custom code) |

## QUESTIONS

> [!tip]- Why doesn't Java force developers to handle RuntimeException?
> RuntimeExceptions typically represent programming errors (like null pointers or invalid indices) that should be fixed rather than caught. Forcing developers to handle these would result in excessive try-catch blocks that obscure code logic. Runtime exceptions can occur almost anywhere, so requiring explicit handling would make code less maintainable. By making them unchecked, Java allows developers to focus on exceptional business conditions while still fixing actual bugs when they're discovered.

> [!warning]- When should you convert between checked and unchecked exceptions?
> Convert checked to unchecked when:
> - The exception crosses an API boundary where clients can't reasonably recover
> - You're implementing an interface that doesn't declare exceptions
> - The exception indicates a programming error rather than an environmental problem
>
> Convert unchecked to checked when:
> - The condition is recoverable and callers should be forced to handle it
> - You're wrapping low-level technical exceptions into domain-specific ones
>
> Always preserve the original exception as the cause when converting between types.

> [!danger]- Design an exception hierarchy for a financial transaction system. What types should be checked vs. unchecked?
> ```
>                     TransactionException (checked)
>                     /        |          \
> InsufficientFundsException   |           TransactionTimeoutException
> (checked)                    |           (checked)
>                              |
>                  TransactionSystemException (unchecked)
>                  /            |            \
> DatabaseConnectionException   |             SecurityViolationException
> (unchecked)              ConfigurationException
>                          (unchecked)
> ```
>
> **Checked exceptions** represent business/domain errors that client code should handle:
> - InsufficientFundsException: Business rule violation, can be handled by requesting more funds
> - TransactionTimeoutException: Temporary condition, can be handled by retrying
>
> **Unchecked exceptions** represent programming or system errors:
> - DatabaseConnectionException: System failure, generally unrecoverable by application code
> - ConfigurationException: Setup error, indicates incorrect deployment
> - SecurityViolationException: Programming error in authorization logic
>
> This design separates business-level failures from system-level failures, allowing for appropriate handling strategies.

- - -

