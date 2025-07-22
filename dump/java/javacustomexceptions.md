# Java -> Exceptions -> Custom Exceptions
## Summary
> [!summary]
> Custom exceptions in Java allow developers to create application-specific error types that communicate domain problems more precisely than generic exceptions. By extending the standard exception classes, developers can add specialized fields and methods that provide contextual information about errors. Creating well-designed custom exceptions improves API clarity, enables targeted exception handling, and enhances debugging by providing more meaningful error information tailored to the application domain.

## When to Create Custom Exceptions

Custom exceptions should be created when:

1. **Domain-specific errors** need to be communicated
2. You need to add **additional context** or behavior to exceptions
3. You want to make your **API more expressive** and self-documenting
4. You need to **group related exceptions** in a hierarchy
5. You want to provide **consistent error handling** across your application

## Creating Basic Custom Exceptions

### Checked Custom Exception

```java
public class FileFormatException extends Exception {
    
    // Basic constructor
    public FileFormatException() {
        super();
    }
    
    // Constructor with message
    public FileFormatException(String message) {
        super(message);
    }
    
    // Constructor with message and cause
    public FileFormatException(String message, Throwable cause) {
        super(message, cause);
    }
    
    // Constructor with cause
    public FileFormatException(Throwable cause) {
        super(cause);
    }
}
```

### Unchecked Custom Exception

```java
public class InvalidProductIdException extends RuntimeException {
    
    public InvalidProductIdException(String message) {
        super(message);
    }
    
    public InvalidProductIdException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

## Adding Context to Custom Exceptions

Adding domain-specific fields enhances the information provided by the exception:

```java
public class InsufficientFundsException extends Exception {
    private final BigDecimal requestedAmount;
    private final BigDecimal availableAmount;
    private final String accountId;
    
    public InsufficientFundsException(String accountId, 
                                      BigDecimal requestedAmount, 
                                      BigDecimal availableAmount) {
        super(String.format("Insufficient funds in account %s: requested %s but only %s available", 
                           accountId, requestedAmount, availableAmount));
        this.accountId = accountId;
        this.requestedAmount = requestedAmount;
        this.availableAmount = availableAmount;
    }
    
    // Getters
    public BigDecimal getRequestedAmount() {
        return requestedAmount;
    }
    
    public BigDecimal getAvailableAmount() {
        return availableAmount;
    }
    
    public String getAccountId() {
        return accountId;
    }
    
    // Utility method
    public BigDecimal getDeficit() {
        return requestedAmount.subtract(availableAmount);
    }
}
```

## Building Exception Hierarchies

Creating hierarchies helps organize related exceptions:

```java
// Base exception for all validation errors
public abstract class ValidationException extends Exception {
    private final String fieldName;
    
    public ValidationException(String fieldName, String message) {
        super(message);
        this.fieldName = fieldName;
    }
    
    public String getFieldName() {
        return fieldName;
    }
}

// Specific validation exceptions
public class RequiredFieldException extends ValidationException {
    public RequiredFieldException(String fieldName) {
        super(fieldName, "Field is required: " + fieldName);
    }
}

public class InvalidFormatException extends ValidationException {
    private final String invalidValue;
    private final String expectedFormat;
    
    public InvalidFormatException(String fieldName, String invalidValue, String expectedFormat) {
        super(fieldName, String.format("Invalid format for field %s: '%s' does not match %s", 
                                      fieldName, invalidValue, expectedFormat));
        this.invalidValue = invalidValue;
        this.expectedFormat = expectedFormat;
    }
    
    // Getters
    public String getInvalidValue() {
        return invalidValue;
    }
    
    public String getExpectedFormat() {
        return expectedFormat;
    }
}
```

## Adding Utility Methods

Custom exceptions can include utility methods to help with error handling:

```java
public class ServiceException extends Exception {
    private final String serviceId;
    private final int errorCode;
    
    public ServiceException(String serviceId, int errorCode, String message) {
        super(message);
        this.serviceId = serviceId;
        this.errorCode = errorCode;
    }
    
    // Getters
    public String getServiceId() {
        return serviceId;
    }
    
    public int getErrorCode() {
        return errorCode;
    }
    
    // Utility methods
    public boolean isRetryable() {
        // Determine if this exception represents a temporary condition
        return errorCode >= 500 && errorCode < 600;
    }
    
    public Map<String, Object> toErrorResponse() {
        // Create a standardized error response format
        Map<String, Object> response = new HashMap<>();
        response.put("service", serviceId);
        response.put("errorCode", errorCode);
        response.put("message", getMessage());
        response.put("timestamp", new Date());
        return response;
    }
}
```

## Serialization Considerations

For exceptions that might cross JVM boundaries (RMI, serialization), include a serialVersionUID:

```java
public class RemoteServiceException extends Exception {
    private static final long serialVersionUID = 1L; // Important for serialization
    
    // Constructors and methods
}
```

## Exception Translation Patterns

Creating methods to translate from one exception type to another:

```java
public class DatabaseException extends Exception {
    // Constructor and fields
    
    // Static factory method translating SQLException to DatabaseException
    public static DatabaseException fromSQLException(SQLException e) {
        int vendorCode = e.getErrorCode();
        String sqlState = e.getSQLState();
        
        if (sqlState != null && sqlState.startsWith("23")) {
            return new ConstraintViolationException("Database constraint violation", e);
        } else if (vendorCode == 1205 || vendorCode == 8) {
            return new DeadlockException("Database deadlock detected", e);
        } else {
            return new DatabaseException("Database error: " + e.getMessage(), e);
        }
    }
}
```

## Best Practices for Custom Exceptions

1. **Use meaningful names** that clearly indicate the error condition
2. **Extend the appropriate base class**:
   - `Exception` for checked exceptions (recoverable conditions)
   - `RuntimeException` for unchecked exceptions (programming errors)
3. **Include all constructors** from the parent class
4. **Add context-specific fields** that help with debugging and recovery
5. **Provide informative error messages** that explain what went wrong
6. **Document exceptions thoroughly** with Javadoc
7. **Keep exceptions serializable** if they might cross JVM boundaries
8. **Create exception hierarchies** when you have families of related errors
9. **Use static factory methods** for common creation patterns
10. **Include cause chaining** to preserve the full error context

## Example: Custom Exception in Action

Usage example showing how custom exceptions can improve error handling:

```java
public class AccountService {
    public void transfer(String fromAccountId, String toAccountId, BigDecimal amount) 
            throws InsufficientFundsException, AccountNotFoundException, AccountFrozenException {
        
        Account fromAccount = accountRepository.findById(fromAccountId)
            .orElseThrow(() -> new AccountNotFoundException(fromAccountId));
        
        Account toAccount = accountRepository.findById(toAccountId)
            .orElseThrow(() -> new AccountNotFoundException(toAccountId));
        
        if (fromAccount.isFrozen()) {
            throw new AccountFrozenException(fromAccountId, "Source account is frozen");
        }
        
        if (toAccount.isFrozen()) {
            throw new AccountFrozenException(toAccountId, "Destination account is frozen");
        }
        
        if (fromAccount.getBalance().compareTo(amount) < 0) {
            throw new InsufficientFundsException(
                fromAccountId, 
                amount, 
                fromAccount.getBalance()
            );
        }
        
        // Perform the transfer
        fromAccount.withdraw(amount);
        toAccount.deposit(amount);
        
        accountRepository.save(fromAccount);
        accountRepository.save(toAccount);
    }
}
```

Client code can then handle these specific exceptions:

```java
try {
    accountService.transfer("123", "456", new BigDecimal("500.00"));
    showSuccessMessage("Transfer completed successfully");
} catch (InsufficientFundsException e) {
    showError("Transfer failed: Insufficient funds",
             "You need $" + e.getDeficit() + " more to complete this transfer");
    offerOverdraftOption(e.getAccountId(), e.getDeficit());
} catch (AccountNotFoundException e) {
    showError("Transfer failed: Account not found", 
             "The account " + e.getAccountId() + " does not exist");
} catch (AccountFrozenException e) {
    showError("Transfer failed: Account frozen",
             "The account " + e.getAccountId() + " is currently frozen");
    offerCustomerServiceContact();
}
```

## Questions

> [!tip]- How do you decide whether to create a checked or unchecked custom exception?
> Choose a checked exception when:
> - The error is recoverable and the client code should be aware of it
> - The exception represents a expected but unusual condition
> - You want to force callers to handle or propagate the exception
> 
> Choose an unchecked exception when:
> - The exception represents a programming error that shouldn't happen
> - Recovery is unlikely or impossible
> - Forcing clients to handle the exception would lead to boilerplate code
>
> For example, a `FileFormatException` might be checked since clients should handle malformed files, while an `InvalidConfigurationException` might be unchecked since it indicates a configuration bug that should be fixed.

> [!warning]- What are the advantages of using custom exceptions over generic ones like Exception or RuntimeException?
> Custom exceptions provide several benefits:
>
> 1. **Domain-specific context:** Custom fields can capture relevant error data like account IDs or transaction amounts
> 2. **Better error filtering:** Allows catching specific error types rather than broad categories
> 3. **Self-documenting code:** The exception name itself communicates what went wrong
> 4. **API clarity:** Method signatures declare the specific error conditions through throws clauses
> 5. **Extra functionality:** Can include helper methods for error handling or compensation
> 6. **Consistent handling:** Related exceptions can share behavior through inheritance
> 7. **Improved logging:** Custom toString() can provide better formatted error messages
>
> Using generic exceptions loses this context and forces callers to rely on string parsing or other brittle mechanisms to determine the nature of errors.

> [!danger]- How would you implement a custom exception system for a multi-layered enterprise application?
> A robust multi-layered exception system might look like:
>
> ```java
> // Base exceptions for each layer
> public abstract class ApplicationException extends Exception {
>     private final String errorCode;
>     private final ErrorSeverity severity;
>     
>     // Constructor, getters, etc.
>     
>     public boolean isLoggable() {
>         return severity.ordinal() >= ErrorSeverity.WARNING.ordinal();
>     }
> }
>
> // Layer-specific base exceptions
> public abstract class DataAccessException extends ApplicationException { /* ... */ }
> public abstract class ServiceException extends ApplicationException { /* ... */ }
> public abstract class UIException extends ApplicationException { /* ... */ }
>
> // Specific exceptions in each layer
> public class EntityNotFoundException extends DataAccessException { /* ... */ }
> public class OptimisticLockingException extends DataAccessException { /* ... */ }
>
> public class BusinessRuleViolationException extends ServiceException { /* ... */ }
> public class ServiceUnavailableException extends ServiceException { /* ... */ }
>
> // Exception translation utilities
> public class ExceptionTranslator {
>     public static ServiceException translateDataException(DataAccessException ex) {
>         if (ex instanceof EntityNotFoundException) {
>             return new ResourceNotFoundException(ex.getEntityName(), ex.getEntityId(), ex);
>         } else if (ex instanceof OptimisticLockingException) {
>             return new ConcurrentModificationServiceException(ex.getEntityName(), ex);
>         }
>         // Other translations...
>         return new GenericServiceException("Data access error", ex);
>     }
> }
> ```
>
> Each layer would:
> 1. Define its own exceptions relevant to that layer's concerns
> 2. Catch exceptions from lower layers and translate them to its own exception types
> 3. Add contextual information before propagating exceptions upward
> 4. Use common base classes to enable consistent handling across the application
>
> This approach preserves the separation of concerns while maintaining the full context of errors through the exception chain.

- - -
#java
