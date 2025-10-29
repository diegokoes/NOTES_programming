# SERVIDOR :D 

## WELCOME FILE
```xml
<welcome-file-list>  
    <welcome-file>welcome.jsp</welcome-file>  
</welcome-file-list>
```
## XLS 
```java
resp.setContentType("application/vnd.ms-excel; charset=UTF-8");
resp.setHeader("Content-Disposition", "attachment; filename=\"productos.xls\"");
req.setAttribute("x", x);
getServletContext().getRequestDispatcher("/.jsp").forward(req, resp);
```
## PARAMETERS 
### LIST'S 
```java
List<String> X = Optional.ofNullable(request.getParameterValues("X")) .map(Arrays::asList) .orElseGet(Collections::emptyList);
```
## LOGGING / ERROR.JSP / STATUS CODES
```java
//logger
private static final Logger log = Logger.getLogger(NOMBRECLASE.class.getName());
//a error.jsp el mensaje
        } catch (Exception e) {
            log.severe(e.getMessage());
            request.setAttribute("error", e.getMessage());
          getServletContext().getRequestDispatcher("/error.jsp").forward(request, response);
            return;
        }
//codes
HttpServletResponse.SC_NOT_FOUND
```
## COOKIE

> in jsp 
${empty cookie.color.value ? "blue" : cookie.color.value}

> create

```java
Cookie cookie = new Cookie("AAAAAA", "AAAAAA");
cookie.setMaxAge(7 * 24 * 60 * 60);
cookie.setPath(request.getContextPath()); 
response.addCookie(cookie);

```

> read

```java
Cookie[] cookies = request.getCookies();
if (cookies != null){
  for (Cookie c : cookies) {
      if (c.getName().equals("usuario")) {
          String nombreUsuario = c.getValue();
          // Realizar alguna acción con el valor de la cookie
          // out.println("Bienvenido de nuevo " + c.getValue());
      }
  }
}
```

> delete

```
Cookie cookie = new Cookie("usuario", "");
cookie.setMaxAge(0);
cookie.setPath(request.getContextPath())
response.addCookie(cookie);
```

## DATE

```java
if (fechaParam != null && !fechaParam.isBlank()) {

try {

var ld = java.time.LocalDate.parse(fechaParam);

fecha = java.sql.Date.valueOf(ld);

} catch (java.time.format.DateTimeParseException dtpe) {

errors.add("La fecha de publicación no tiene el formato yyyy-MM-dd.");

}

}
```

## INIT
```java
@Override  
public void init() throws ServletException {  
    super.init();  
    try {  
        daoP = new ProductoDAO();  
    } catch (Exception e) {  
        throw new ServletException("Error inicializando DAOs", e);  
    }  
}
```