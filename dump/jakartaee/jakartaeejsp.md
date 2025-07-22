# JakartaEE: JSP
## Summary
> [!summary]
> ***JavaServer Pages*** allows to insert Java code into HTML to render content dinamicly server-side.
>
> For the ==**View**== part of the application in an [[mvc|MVC]] app.

## Theory

**Related**: [[jakartaeeexpressionlanguage|Expression Language]]


When a JSP page is requested for the first time, it is translated into a [[jakartaeeservlets|sevlet]], compiled into a `.class` file, and executes the `service()` method of the servlet to generate the response. Subsequent requests reuse the compilted servlet for better performance.

### Directives
```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
```

### Importing in JSP
JSP allows importing Java classes using the page directive. This is useful for utilizing various Java classes in the JSP page. For example:
```java
<%@ page import="java.util.List, java.util.ArrayList" %>
```
You can import multiple classes by separating them with commas or use wildcards to import an entire package.

### Using Tag Libraries
Custom tag libraries, such as JSTL, enhance JSP functionality by providing standardized tags for common tasks without embedding Java code directly. To use a tag library, first declare it with the taglib directive:
```java
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```
This enables you to use JSTL core tags like `<c:if>`, `<c:forEach>`, etc., to simplify page logic.

JavaServer Pages (JSP) provide three main constructs for integrating Java code into web pages. Each has a specific purpose and operates differently within the JSP page lifecycle.

| Feature                | Declarations `<%! %>`                | Scriptlets `<% %>`                     | Expressions `<%= %>`                 |
|------------------------|--------------------------------------|----------------------------------------|--------------------------------------|
| **Purpose**            | Define class-level variables/methods | Execute Java code during processing    | Display dynamic values in output     |
| **Scope**              | Entire servlet                       | Local to the `_jspService()` method    | Single evaluation                    |
| **Persistence**        | Between requests                     | Only during the current request        | Only during the current request      |
| **Can define methods** | Yes                                  | No                                     | No                                   |
| **Result**             | No direct output                     | Can generate output via `out.print()`  | Automatically generates output       |


### JavaBeans in JSP: Good Practices with Standard Tags

Using JavaBeans with JSP standard tags provides a cleaner, more maintainable approach to web development compared to using scriptlets. Here's a rundown of the best practices and important tags:

#### Key JSP Standard Tags for JavaBeans

1. **`<jsp:useBean>`** - Locates or creates a JavaBean instance:
    
    ```jsp
    <jsp:useBean id="beanName" class="package.ClassName" scope="scopeValue"/>
    ```
    
    - `id`: The name used to reference the bean
    - `class`: The fully qualified class name
    - `scope`: Where the bean will be available (page, request, session, application)
2. **`<jsp:setProperty>`** - Sets bean property values:
    
    ```jsp
    <!-- Set specific property with a value -->
    <jsp:setProperty name="beanName" property="propertyName" value="value"/>
    
    <!-- Set property from request parameter -->
    <jsp:setProperty name="beanName" property="propertyName" param="paramName"/>
    
    <!-- Automatically set all properties from matching request parameters -->
    <jsp:setProperty name="beanName" property="*"/>
    ```
    
3. **`<jsp:getProperty>`** - Retrieves and outputs a bean property value:
    
    ```jsp
    <jsp:getProperty name="beanName" property="propertyName"/>
    ```
    

#### Benefits of Using Standard Tags

- Clearer separation between Java code and HTML/presentation
- Declarative, more readable syntax
- Explicit control over bean scope
- Less code to achieve the same functionality
- Better long-term maintainability
- Automatic parameter mapping for form processing





## Questions
> [!tip]- Where do JSP files go in the project?
> JSP files typically go in the web content directory of a Java web application. In a standard Java EE or Jakarta EE project structure, JSP files are usually placed in:
>
>- The root web directory (often called `WebContent` or `webapp`)
>- Or organized in subdirectories within this root for better structure
>
>In a Maven or Gradle project, the standard location would be:
>
>- `src/main/webapp/` - The root web content directory
>- `src/main/webapp/WEB-INF/` - Protected directory (not directly accessible)
>- `src/main/webapp/WEB-INF/jsp/` - Common practice to store JSP files used as views
>
>JSP files placed directly in the webapp directory are publicly accessible through their URL path, while those in WEB-INF are protected and can only be accessed through server-side forwards or includes.

> [!warning]- What is the difference in handling state management with JSF?
> **JSP**:
>
>Stateless: Each request is independent, and maintaining state requires explicit handling (e.g., using sessions).
>Control: Developers manage state manually, which can lead to increased complexity.
>**JSF**:
>
>Stateful: Maintains the state of UI components between requests automatically.
Built-in Support: Facilitates complex interactions and component states without additional coding.

> [!danger]- Explain how a custom tag library's lifecycle works in JSP, including how tag handlers interact with the JSP page during request processing. What are the key methods that must be implemented, and how does tag pooling impact the design of tag handlers?
> The lifecycle of a custom tag in JSP involves several phases and interactions between the JSP container and the tag handler class:
> 
> 1. **Tag Handler Class Loading**: When a JSP page using custom tags is first requested, the container loads the tag handler classes specified in the TLD (Tag Library Descriptor).
>     
> 2. **Tag Instance Creation**: The container creates instances of tag handlers. If tag pooling is enabled (the default behavior), the container may create a pool of tag handler instances to be reused across multiple requests.
>     
> 3. **Lifecycle Methods Execution**: During page processing, the container invokes the following methods on the tag handler:
>     
>     - **setPageContext(PageContext)**: Sets the PageContext for the tag, providing access to the JSP environment.
>     - **setParent(Tag)**: Sets the parent tag if this tag is nested within another custom tag.
>     - **setAttributes()**: Container calls setter methods for each attribute specified in the tag.
>     - **doStartTag()**: Called when the opening tag is encountered. Returns EVAL_BODY_INCLUDE to process the body, EVAL_BODY_BUFFERED (for BodyTag) to evaluate and capture the body, or SKIP_BODY to ignore the body.
>     - **doInitBody()**: For BodyTag implementations, called after body content is set but before body evaluation.
>     - **doAfterBody()**: Called after the body content has been processed. Returns EVAL_BODY_AGAIN to reevaluate the body or SKIP_BODY to proceed.
>     - **doEndTag()**: Called when the closing tag is encountered. Returns EVAL_PAGE to continue processing the page or SKIP_PAGE to skip the rest.
>     - **release()**: Called to reset the tag handler to its initial state before returning it to the pool.
> 4. **Tag Pooling Impact**: Since tag handlers may be reused across multiple requests via tag pooling, they must be designed with specific considerations:
>     
>     - All instance variables must be reset in the release() method
>     - Tag handlers should not maintain state between invocations
>     - Resource cleanup must happen in doEndTag() or release()
>     - Thread safety becomes critical as handlers might be used concurrently
> 
> To disable tag pooling, you can set the `body-content` attribute to `empty` in the TLD file or implement the `javax.servlet.jsp.tagext.PoolingTagSupport` interface and return false from the isPoolable() method.
> 
> Tag libraries can implement either simple Tag interface (for basic tags), BodyTag interface (for tags that need to manipulate their body content), or the newer SimpleTag interface (introduced in JSP 2.0 with a simplified lifecycle).
> 
> This entire mechanism allows JSPs to be extended with custom functionality while maintaining the performance benefits of reusing objects across requests.


- - - 
#jakartaee  [Expession Language (EL)](jakartaeeexpressionlanguage.md)