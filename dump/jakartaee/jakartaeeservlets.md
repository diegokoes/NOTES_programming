# JAKARTAEE -> SERVLET
## SUMMARY
> [!summary]

## THEORY
# SERVLET LIFECYCLE AND ARCHITECTURE IN JAKARTA EE

Let me walk you through how servlets work in Jakarta EE, breaking down the lifecycle, threading model, and configuration aspects.

## SERVLET LIFECYCLE

A servlet goes through three main phases during its existence:

1. **Initialization (init)**: When the servlet container (like Tomcat) loads your application, it creates exactly one instance of each servlet class and calls its `init()` method. This happens only once in the servlet's lifetime, making it perfect for one-time setup operations like:
    
    - Loading configuration
    - Establishing database connections
    - Initializing expensive resources
2. **Service**: After initialization, the servlet handles requests. For each client request:
    
    - The container creates a new thread
    - The container calls `service()`, which dispatches to methods like `doGet()` or `doPost()`
    - The thread completes when the response is sent
3. **Destruction (destroy)**: When the server shuts down or the servlet is undeployed, the container calls `destroy()`. This is your chance to release resources, close connections, and perform cleanup.
    

## SERVLET INSTANCES AND THREADING

This is a crucial point to understand: **A servlet instance is shared across all requests**. One servlet object serves multiple users simultaneously, with each request running in a separate thread.

```java
public class MyServlet extends HttpServlet {
    private int counter = 0; // Shared across all requests!
    
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        counter++; // This modification affects all users
        // ...
    }
}
```

## SERVLET STATE MANAGEMENT

Because servlets are shared, you should generally **avoid storing user-specific state in servlet instance variables**. There are several better options:

1. **HttpSession**: For user-specific data that persists across requests
    
    ```java
    HttpSession session = request.getSession();
    session.setAttribute("username", username);
    ```
    
2. **Request attributes**: For data that only needs to live during the current request
    
    ```java
    request.setAttribute("temporaryData", someValue);
    ```
    
3. **ServletContext**: For application-wide shared data
    
    ```java
    getServletContext().setAttribute("globalCounter", counter);
    ```
    

## THREADING AND CONCURRENCY

Since multiple threads can execute the same servlet code simultaneously:

- Servlet instance variables are shared across all requests
- Local variables within methods are thread-safe (each thread gets its own)
- Be cautious with instance variables - use synchronization or thread-safe collections when needed

Think of a servlet as an office with multiple customer service representatives (threads). The office (servlet) is one location, but many representatives can help different customers concurrently.

## @WEBSERVLET AND INITIALIZATION PARAMETERS

The `@WebServlet` annotation configures servlet properties without needing XML deployment descriptors. The `initParams` attribute lets you define configuration parameters:

```java
@WebServlet(
    urlPatterns = "/hello", 
    initParams = {
        @WebInitParam(name = "greeting", value = "Hello"),
        @WebInitParam(name = "language", value = "English")
    }
)
public class GreetingServlet extends HttpServlet {
    // ...
}
```

### ACCESSING INIT PARAMETERS

You can access these parameters from within your servlet using:

```java
@Override
public void init() throws ServletException {
    String greeting = getInitParameter("greeting");
    String language = getInitParameter("language");
    // Use these values for servlet configuration
}
```

### COMMON USES FOR INIT PARAMETERS

Init parameters are perfect for servlet-specific configuration that:

1. Shouldn't be hardcoded
2. Might change between deployments
3. Configures the servlet's behavior

Examples include:

- Database connection details
- External service URLs
- Default settings
- Feature flags
- Resource paths

## PUTTING IT ALL TOGETHER

Here's a mental model: A servlet is like a service desk at a government office:

1. **Initialization**: The office opens once in the morning (init)
2. **Service**: Throughout the day, many customers (requests) are served by different clerks (threads)
3. **State Management**:
    - The office layout (servlet instance) is shared
    - Each customer gets their own paperwork (request/response objects)
    - Customer files are stored in a file cabinet (HttpSession)
4. **Destruction**: At the end of the day, the office closes (destroy)

Would you like me to dive deeper into any particular aspect of servlet architecture or provide a complete code example demonstrating these concepts?
## QUESTIONS
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer
- - - 
#jakartaee 