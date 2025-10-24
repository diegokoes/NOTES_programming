# SERVIDOR :D 

## Welcome file
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
## Parameters 
### List's 
```java
List<String> X = Optional.ofNullable(request.getParameterValues("X")) .map(Arrays::asList) .orElseGet(Collections::emptyList);
```
## Logging / error.jsp / status codes
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
response.addCookie(cookie);
cookie.setPath(request.getContextPath());
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

## Date

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





