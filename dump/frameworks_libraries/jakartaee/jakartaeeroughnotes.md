# JAKARTAEE

!TODO , JSON, COMPARATOR ORDENAR.. EN UTILS? IMPLEMENTING COMPARABLE? METODOS DE LIST, ARRAYLIST?
<
## CONFIGS 

config de la 


## JSON

!todo leer un json, enviar un json...

### OBJECT TO JSON

### RETURNING JSON

### JSON TO OBJECT

### JSON TO COLLECTION


## ERRORS

	response.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Lo sentimos no esta autorizado para ingresar a esta página!");
            
            //response.sendError(HttpServletResponse.SC_UNAUTHORIZED);


## COOKIES

- Create

```java
Cookie cookieName = new Cookie("username","diego");
response.addCookie(cookieName);
```

- Read
```java
String username = Optional.ofNullable(request.getCookies())    // wrap possible null
    .map(Arrays::stream)                                       // convert to Stream<Cookie>
    .orElseGet(Stream::empty)                                  // fallback to empty stream
    .filter(c -> "username".equals(c.getName()))
    .map(Cookie::getValue)
    .findFirst()
    .orElse(null);

        
// Forma 1: API Stream
Cookie[] cookies = req.getCookies() != null ? req.getCookies() : new Cookie[0];
return Arrays.stream(cookies)
             .filter(c -> "username".equals(c.getName()))
             .map(Cookie::getValue)
             .findAny();

```
- Delete
```java
Cookie cookieName = new Cookie("username", "");
cookieName.setMaxAge(0);
response.addCookie(cookieName);
```

- Path:
```java
  cookie.setPath("/");
```
## ORDENAR CON COMPARETO

la clase debe implementar Comparable `<objeto>` 


## SESSION

// In a servlet method (doGet/doPost):
//  - request.getSession() : returns the current session, or creates one if none exists.
//  - request.getSession(false) : returns the current session, or null if none exists.
HttpSession session = request.getSession();         
// (equivalent to request.getSession(true))


Casting is required when reading attributes:
```java
User user = (User) session.getAttribute("user");

// Store
session.setAttribute("username", "diego");

// Retrieve
String username = (String) session.getAttribute("username");

// Store an int
session.setAttribute("age", 30);        // autoboxes to Integer.valueOf(30)

// Retrieve
Integer ageObj = (Integer) session.getAttribute("age");
int age = ageObj.intValue();            // unbox back to int
// or simply
int age2 = (Integer) session.getAttribute("age");

// Store a List<String>
List<String> roles = new ArrayList<>();
roles.add("admin");
roles.add("user");
session.setAttribute("roles", roles);

// Retrieve
@SuppressWarnings("unchecked")
List<String> sessionRoles = (List<String>) session.getAttribute("roles");


```

// Delete just this attribute:
session.removeAttribute("username");

// Invalidate the entire session: removes all attributes and marks it invalid.
session.invalidate();

|Method|Returns / Does|
|---|---|
|`session.getId()`|`String` — the unique session identifier (e.g. sent as JSESSIONID cookie)|
|`session.getCreationTime()`|`long` — timestamp (ms since epoch) when session was created|
|`session.getLastAccessedTime()`|`long` — timestamp of last client request that accessed this session|
|`session.getMaxInactiveInterval()`|`int` — timeout in seconds before session auto-invalidation (default ≈30m)|
|`session.setMaxInactiveInterval(int seconds)`|set custom timeout interval|
|`session.isNew()`|`boolean` — `true` if client has not yet acknowledged this session ID|

## FILTERS
> @WebFilter("/*")                        // All requests (root + sub-paths)
> @WebFilter("/products/*")              // /products and anything under it
> @WebFilter({"/products/*", "/shop/*"}) // Multiple paths
> @WebFilter("/exactPath")               // Only matches "/exactPath

```java
> @WebFilter("/path/*")
> public class MyFilter implements Filter {
> @Override
> public void init(FilterConfig filterConfig) throws ServletException {
>     String filterName = filterConfig.getFilterName(); // e.g., "ContadorVisitasFilter"
>     ServletContext context = filterConfig.getServletContext();
>     context.log("Filter " + filterName + " initialized.");
>     
> HttpServletRequest req = (HttpServletRequest) request;
> HttpServletResponse res = (HttpServletResponse) response;


> public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
>         throws IOException, ServletException
> res.sendRedirect("/login");
> // O
> request.getRequestDispatcher("/login.jsp")
>        .forward(request, response);



```
## LISTENERS

- Context
> @WebListener
> public class AppListener implements ServletContextListener {
>     @Override
>     public void contextInitialized(ServletContextEvent sce) {
>         ServletContext sc = sce.getServletContext();
>         sc.log("App started"); 
>         sc.setAttribute("mensaje", "Global value!");
>     }
>
>     @Override
>     public void contextDestroyed(ServletContextEvent sce) {
>         sce.getServletContext().log("App shutdown");
>     }
> }

- Session
> @WebListener
> public class SessionListener implements HttpSessionListener {
>
>     @Override
>     public void sessionCreated(HttpSessionEvent se) {
>         System.out.println("Session created: " + se.getSession().getId());
>     }
>
>     @Override
>     public void sessionDestroyed(HttpSessionEvent se) {
>         System.out.println("Session destroyed: " + se.getSession().getId());
>     }
> }

- Request
> @WebListener
> public class AppListener implements ServletRequestListener {
>     @Override
>     public void requestInitialized(ServletRequestEvent sre) {
>         sre.getServletRequest().setAttribute("mensaje", "request scope value");
>     }
>
>     @Override
>     public void requestDestroyed(ServletRequestEvent sre) {
>         System.out.println("Request destroyed");
>     }
> }

- log :
```java
@Override
public void sessionCreated(HttpSessionEvent se) {
    ServletContext sc = se.getSession().getServletContext();
    sc.log("Session created: " + se.getSession().getId());
}

```

## STREAM EJEMPLOS

- buscar por id 
```java
@Override
public Optional<Producto> buscarPorId(Long id) {
	return listar().stream().filter(p-> p.getId().equals(id)).findAny(); //findAny o findFirst para el Optional
}
```

## BEANS
**OBLIGATORIO SCOPE** 
@SessionScoped

@Named -> Si no se especifica -> public class Carro = mismo nombre que clase con minus 'carro' o ("miCarro")
**Obligatorio** implementar serializable
**Obligatorio** que tenga un constructor vacío


## MODELS
```java
@Entity  !!
@Table(name="fabricante") <- nombre de la tabla
public class Fabricante implements Serializable !! {
@Id
@Column(name="codigo")
private int codigo; -- no haria falta, mismo nombre



¡¡Constructor público sin argumentos!!
¡¡ Getters y Setters para cada propiedad que quiera mapear!!
}
```

## EXCEPTIONS 
```java
package es.daw.web.exceptions;

public class JPAException extends Exception{
    public JPAException(String message){
        super(message);
    }
}
```
## JSF

<h:panelGroup  rendered="#{ condicional para que se renderice o no}">
## PERSISTENCE 
`No es obligatorio poner el <class> ? `
persistence.xml
          `<property name="jakarta.persistence.schema-generation.database.action" value="none"/>` value="validate" para que no de errores



## RETO

lo mismo que el enunciado de la ordinaria, pero con JSF, Y CDI BEAN


- - - 
