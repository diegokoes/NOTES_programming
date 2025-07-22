# JakartaEE -> JSP -> Expression Language

## Summary

> [!summary] 
> Expression Language (EL) is a simplified language used to access and manipulate data within JSP pages. It provides an elegant and concise way to access data stored in various objects such as requests, sessions, applications, and JavaBeans.
> 
> When working with web applications, we encounter numerous objects that store data. Without EL, retrieving this data requires writing multiple lines of code with specific syntax for each object type:
> 
> - To access request parameters, we must use _**request.getParameter()**_
> - To retrieve data from a session, we need to use _**session.getAttribute()**_
> - Similar verbose syntax exists for other scopes and objects
> 
> Expression Language solves this verbosity by providing a unified, intuitive syntax to access all these data sources directly. With EL, developers can write cleaner, more readable code while maintaining the same functionality.

## Theory

Expression Language (EL) is a crucial component of JSP that simplifies data access across different scopes of a web application. It was introduced to reduce the amount of Java code embedded within JSP pages, making them more maintainable and readable.

### Key EL Implicit Objects

1. **param**
    
    - Represents request parameters (equivalent to request.getParameter())
    - Access syntax: `${param.parameterName}`
    - Example: For a URL like "page.jsp?id=123", you can access the id using `${param.id}`
2. **paramValues**
    
    - Used for parameters with multiple values (like checkboxes with the same name)
    - Access syntax: `${paramValues.parameterName[index]}`
    - Example: For multiple colors selected, access them with `${paramValues.colors[0]}`, `${paramValues.colors[1]}`, etc.
3. **header**
    
    - Provides access to HTTP request headers
    - Access syntax: `${header["Header-Name"]}` or `${header.headerName}`
    - Example: Get user's browser info with `${header["User-Agent"]}`
4. **headerValues**
    
    - For headers with multiple values
    - Access syntax: `${headerValues.headerName[index]}`
5. **cookie**
    
    - Allows access to cookies sent with the request
    - Access syntax: `${cookie.cookieName.value}`
    - Example: Retrieve user preferences with `${cookie.userPrefs.value}`
6. **initParam**
    
    - Accesses initialization parameters defined in @WebServlet or web.xml
    - Access syntax: `${initParam.paramName}`
7. **sessionScope**
    
    - Accesses attributes in the session scope (set via session.setAttribute())
    - Access syntax: `${sessionScope.attributeName}`
8. **session**
    
    - Direct access to the session object
    - Example: Get session ID with `${session.id}`
9. **requestScope**
    
    - Accesses attributes in the request scope (set via request.setAttribute())
    - Access syntax: `${requestScope.attributeName}`
10. **request**
    
    - Direct access to the HttpServletRequest object
    - Example: Get client IP with `${request.remoteAddr}`
11. **applicationScope**
    
    - Accesses attributes in application scope (set via application.setAttribute())
    - Access syntax: `${applicationScope.attributeName}`
12. **application**
    
    - Access to ServletContext
    - Example: Get context path with `${application.contextPath}`
13. **response**
    
    - Direct access to HttpServletResponse (rarely used in EL)
    - Example: Check content type with `${response.contentType}`
14. **exception**
    
    - Access to exceptions thrown in error pages
    - Example: Get error message with `${exception.message}`

### Attribute Resolution in EL

When you use a variable name without specifying a scope (e.g., `${username}`), EL searches for that attribute in this order:

1. Page scope
2. Request scope
3. Session scope
4. Application scope

This automatic resolution makes code cleaner while maintaining flexibility.

## Questions

> [!tip]- What is the difference between `${param.name}` and `${paramValues.name}`? 
> `${param.name}` retrieves a single parameter value, suitable for form fields that submit a single value (like text inputs or radio buttons). `${paramValues.name}` returns an array of values and is used for form fields that can submit multiple values with the same name (like checkboxes or multi-select lists). For example, if you have multiple checkboxes with the name "hobbies", you would access them as `${paramValues.hobbies[0]}`, `${paramValues.hobbies[1]}`, etc.

> [!warning]- Why doesn't `${username}` require specifying a scope like `${sessionScope.username}`? 
> EL provides automatic scope resolution when you use an unqualified attribute name like `${username}`. When you use this syntax, EL searches for the attribute in this specific order: page scope, request scope, session scope, and finally application scope. It returns the first match it finds.
> 
> However, using the specific scope (`${sessionScope.username}`) is recommended when:
> 
> 1. You have the same attribute name in multiple scopes and need the specific one
> 2. You want to make your code more explicit about where data is coming from
> 3. You want to avoid potential scope resolution conflicts

> [!danger]- What happens if I try to access a non-existent attribute with EL? 
> Unlike Java code that would throw a NullPointerException, Expression Language handles missing attributes gracefully. When you try to access a non-existent attribute with EL (e.g., `${nonExistentValue}`), it simply returns an empty string rather than throwing an exception.
> 
> This behavior makes JSP pages more robust, but it can also hide errors. For properties of objects, attempting to access a non-existent property will also return an empty string rather than throwing an exception. If you need to check if a value exists before using it, you can use the empty operator: `${empty attributeName}`.

> [!tip]- How can I perform calculations or operations in Expression Language? 
> Expression Language supports various operators for calculations and operations:
> 
> 1. Arithmetic operators: +, -, *, /, % (modulo), div, mod Example: `${num1 + num2}` or `${price * quantity}`
>     
> 2. Comparison operators: ==, !=, <, >, <=, >=, eq, ne, lt, gt, le, ge Example: `${score >= 80}` or `${user.role eq 'admin'}`
>     
> 3. Logical operators: &&, ||, !, and, or, not Example: `${user.age >= 18 && user.verified == true}`
>     
> 4. Conditional operator: ? : (ternary) Example: `${empty username ? 'Guest' : username}`
>     
> 
> These operations make EL powerful for not just accessing data but also processing it directly in your JSP pages.