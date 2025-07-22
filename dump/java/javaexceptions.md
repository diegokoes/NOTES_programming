# Java -> Exceptions

## Related Notes
- [Java Exception Hierarchy](javaexceptionhierarchy.md)
- [Creating Custom Exceptions](javacustomexceptions.md)

## Summary
> [!summary]
> Exceptions in Java are objects that represent exceptional events disrupting normal program flow. When an error occurs, Java creates an exception object containing information about the error and searches the call stack for appropriate handlers. Java has checked exceptions (must be caught or declared) and unchecked exceptions (no explicit handling required). The exception mechanism uses try-catch-finally blocks for handling, throws clauses for declaring, and the throw keyword for raising exceptions. This approach separates error handling from regular code, enables error propagation up the call stack, and provides type-based categorization of errors.

## Theory

### What Are Exceptions?

An exception is an event that disrupts the normal flow of a program's instructions. When something unexpected happens during program execution, Java creates an object (the exception) containing error details and the state of program execution.

### Exception Hierarchy

All exceptions derive from `java.lang.Throwable`, which has two main subclasses:

1. **Error** - Represents serious problems external to the application (e.g., `OutOfMemoryError`)
2. **Exception** - Represents conditions applications should handle
   - **Checked Exceptions** - Must be handled or declared (e.g., `IOException`)
   - **Unchecked Exceptions** - Subclasses of `RuntimeException` (e.g., `NullPointerException`)

### Types of Exceptions

#### Checked Exceptions
- Must follow "catch or specify" requirement
- Compile-time enforcement
- Examples: `IOException`, `SQLException`
- Used for recoverable conditions

#### Unchecked Exceptions
- **Runtime Exceptions**: Represent programming errors (e.g., `NullPointerException`, `ArrayIndexOutOfBoundsException`)
- **Errors**: Represent serious system problems (e.g., `OutOfMemoryError`)
- Not subject to "catch or specify" requirement
- Examples: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException`

### Exception Handling

#### try-catch Block
```java
try {
    // Code that might throw exceptions
} catch (ExceptionType1 name1) {
    // Handler for ExceptionType1
} catch (ExceptionType2 name2) {
    // Handler for ExceptionType2
}
```

#### Multi-catch (Java 7+)
```java
try {
    // Code that might throw exceptions
} catch (IOException | SQLException ex) {
    // Handler for either exception type
}
```

#### finally Block
```java
try {
    // Code that might throw exceptions
} catch (Exception e) {
    // Exception handling
} finally {
    // Code that always executes
    // Used for cleanup (closing files, resources, etc.)
}
```

#### try-with-resources (Java 7+)
Automatically closes resources implementing `AutoCloseable` or `Closeable`:

```java
try (BufferedReader br = new BufferedReader(new FileReader(path))) {
    return br.readLine();
} // br is automatically closed
```

### Throwing Exceptions

#### throw Statement
```java
if (accountBalance < amount) {
    throw new InsufficientFundsException("Not enough money in account");
}
```

#### throws Clause
Used to declare checked exceptions a method might throw:

```java
public void readFile() throws IOException {
    // Method code
}
```

### Chained Exceptions

Preserving the original cause of an exception:

```java
try {
    // Code
} catch (IOException e) {
    throw new ServiceException("Service failed", e); // e is the cause
}
```

Accessing the cause:
```java
Throwable cause = exception.getCause();
```

### Creating Custom Exceptions

Basic custom exception:
```java
public class CustomException extends Exception {
    public CustomException() { super(); }
    public CustomException(String message) { super(message); }
    public CustomException(String message, Throwable cause) { super(message, cause); }
}
```

### Stack Trace Information

```java
try {
    // Code
} catch (Exception e) {
    e.printStackTrace();
    // Or for more control:
    StackTraceElement[] elements = e.getStackTrace();
    for (StackTraceElement element : elements) {
        System.err.println(element.getFileName() + ":" + element.getLineNumber()
                            + " > " + element.getMethodName());
    }
}
```

### Advantages of Exceptions

1. **Separation of concern** - Regular code vs error handling code
2. **Error propagation** - Errors can bubble up to the appropriate handler
3. **Type grouping** - Exceptions are organized in a class hierarchy
4. **Clean code** - Improves readability and maintainability

### Best Practices

- **Be specific with catch blocks**: Catch the most specific exceptions rather than generic ones to handle different error scenarios appropriately.
- **Don't swallow exceptions**: Avoid empty catch blocks. Always log or handle exceptions meaningfully.
- **Use meaningful exception messages**: Include relevant details that help diagnose the issue.
- **Use custom exceptions for domain-specific errors**: Create specialized exceptions that represent your application's error states.
- **Always clean up resources**: Use try-with-resources for AutoCloseable resources or clean up in finally blocks.
- **Prefer unchecked exceptions for programmatic errors**: Use RuntimeException subclasses for errors that indicate programming mistakes.
- **Use checked exceptions for recoverable conditions**: When the caller should be able to handle the exception.
- **Document exceptions in Javadoc**: Use @throws tags to document both checked and significant unchecked exceptions.
- **Include the cause**: When wrapping exceptions, always include the original exception as the cause.
- **Keep exception handling close to the source**: Handle exceptions at the level where you have enough context to make appropriate decisions.
- **Use exception hierarchies**: Group related exceptions to allow catching categories of exceptions.
- **Test exception scenarios**: Include unit tests for exception paths, not just the happy path.

## Questions
> [!tip]- What is the difference between checked and unchecked exceptions?
> Checked exceptions must be either caught (with try-catch) or declared (with throws) at compile time. They extend Exception but not RuntimeException, and represent recoverable conditions that good programs should handle. Unchecked exceptions include RuntimeException and its subclasses plus Error types. They don't require explicit handling or declaration and typically represent programming errors that can't be reasonably recovered from at runtime.

> [!tip]- Explain the purpose of the finally block in exception handling.
> The finally block contains code that always executes when a try block exits, whether normally or due to an exception. It's primarily used for cleanup operations like closing resources (files, connections, etc.) to prevent resource leaks. It executes even if the try or catch blocks contain a return, continue, or break statement. The only scenarios where finally won't execute are if the JVM exits or the thread executing the try block is interrupted or killed.

> [!warning]- How does try-with-resources differ from traditional try-catch-finally?
> Try-with-resources (Java 7+) automatically closes resources that implement AutoCloseable or Closeable interfaces. Unlike traditional try-catch-finally where resources must be closed manually in the finally block, try-with-resources handles resource closing automatically and in the correct order. It also manages suppressed exceptions better: if an exception occurs in the try block and another during closing resources, the latter is suppressed but accessible via getSuppressed(). This leads to cleaner, safer code without the boilerplate of manual resource management.

> [!warning]- How would you implement exception chaining and why is it useful?
> Exception chaining preserves the original cause when throwing a new exception:
> ```java
> try {
>     // Low-level operation
> } catch (LowLevelException e) {
>     throw new HighLevelException("Operation failed", e);
> }
> ```
> It's useful because it preserves the complete error context. Higher-level code can catch domain-specific exceptions while still having access to the underlying technical cause, enabling both user-friendly error reporting and detailed diagnostic information for troubleshooting. The cause chain can be accessed via getCause() or printed with printStackTrace().

> [!danger]- Design a custom exception hierarchy for a banking system. Explain your design decisions and how they would improve error handling.
> A robust banking exception hierarchy might look like:
> ```java
> // Base exception
> public abstract class BankingException extends Exception {
>     private final String errorCode;
>     
>     public BankingException(String errorCode, String message) {
>         super(message);
>         this.errorCode = errorCode;
>     }
>     
>     public BankingException(String errorCode, String message, Throwable cause) {
>         super(message, cause);
>         this.errorCode = errorCode;
>     }
>     
>     public String getErrorCode() { return errorCode; }
> }
>
> // Account exceptions
> public class AccountException extends BankingException {
>     // Constructor implementations
> }
>
> public class InsufficientFundsException extends AccountException {
>     private final BigDecimal requested;
>     private final BigDecimal available;
>     
>     // Constructor and getters
> }
>
> // Transaction exceptions
> public class TransactionException extends BankingException {
>     // Constructor implementations
> }
>
> public class TransactionTimeoutException extends TransactionException {
>     // Constructor implementations
> }
> ```
>
> Design decisions:
> 1. Base abstract class with error codes for consistent logging and reporting
> 2. Domain-specific branches (Account, Transaction) for logical organization
> 3. Specific exceptions with contextual data (requested/available amounts)
> 4. All checked exceptions since banking errors should be handled explicitly
>
> This improves error handling by enabling:
> - Precise exception filtering with specific catch blocks
> - Rich error context for better debugging
> - Consistent error codes for support and logging
> - Domain-specific grouping for handling related errors together
> - Clear API contracts showing what can go wrong

> [!danger]- How would you implement a retry mechanism for handling transient exceptions in a distributed system?
> An effective retry mechanism for transient failures:
>
> ```java
> public <T> T executeWithRetry(Callable<T> task, 
>                              List<Class<? extends Exception>> retryableExceptions,
>                              int maxAttempts, 
>                              long initialDelay,
>                              double backoffMultiplier) throws Exception {
>     
>     int attempts = 0;
>     long delay = initialDelay;
>     Exception lastException = null;
>     
>     while (attempts < maxAttempts) {
>         try {
>             return task.call();
>         } catch (Exception e) {
>             lastException = e;
>             
>             // Check if we should retry this exception
>             boolean shouldRetry = retryableExceptions.stream()
>                 .anyMatch(exClass -> exClass.isInstance(e));
>             
>             if (!shouldRetry || attempts >= maxAttempts - 1) {
>                 throw e;
>             }
>             
>             // Log the failure
>             log.warn("Operation failed (attempt {} of {}), retrying in {}ms: {}",
>                     attempts + 1, maxAttempts, delay, e.getMessage());
>                     
>             // Wait before retry with exponential backoff
>             try {
>                 Thread.sleep(delay);
>                 delay = Math.round(delay * backoffMultiplier);
>                 attempts++;
>             } catch (InterruptedException ie) {
>                 Thread.currentThread().interrupt();
>                 throw e; // Propagate original exception
>             }
>         }
>     }
>     
>     // This should never happen due to the logic above
>     throw new IllegalStateException("Retry failed", lastException);
> }
> ```
>
> Usage example:
> ```java
> try {
>     String result = executeWithRetry(
>         () -> serviceClient.getData(),        // operation to perform
>         Arrays.asList(                        // retryable exceptions
>             ConnectException.class, 
>             SocketTimeoutException.class,
>             ServiceUnavailableException.class
>         ),
>         3,                                    // max attempts
>         1000,                                 // initial delay (1s)
>         2.0                                   // exponential backoff factor
>     );
>     processResult(result);
> } catch (Exception e) {
>     // Handle ultimate failure
> }
> ```
>
> This implementation includes:
> - Configurable retry count and exceptions
> - Exponential backoff to prevent overwhelming the system
> - Proper handling of thread interruption
> - Detailed logging of retry attempts
> - Generic approach that works with any operation

- - - 
#java