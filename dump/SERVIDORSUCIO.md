melola@educa.madrid.org


no cammelcase para invocar server. name= "parameters-get"


si quitamos ioexception o throws, o manejarla con try catch![[Pasted image 20240916191335.png]]
	y si hubiera un problema? no llega al CLOSE!!! mal mal, mejor usar un
	try con recursos 
	
        PrintWriter out = null; para el finally
        
   cerrar un pointer null? -> nullpointerexception![[Pasted image 20240916191639.png]]
	si es null no close.

La segunda forma para hacer eso : *try con recursos* 

![[Pasted image 20240916191839.png]]
try (parentesis) donde creamos el objeto/inicializamos dentro del bloque en paréntesis, pase lo qu pase lo cierra. Averiguar por qué. 

,,
# 17/09/2024


![[Pasted image 20240918160518.png]]
![[Pasted image 20240918160533.png]]




# 19/09/2024
```
@WebServlet(
    urlPatterns = {"/init", "/init-servlet"},
    name = "inicio",
    initParams =
    {
        @WebInitParam(name = "peticiones", value = "5"),
        @WebInitParam(name = "saveDir", value = "D:/FileUpload"),
        @WebInitParam(name = "allowedTypes", value = "jpg,jpeg,gif,png")
    }
)
```


# 20/09/2024

initservlet, dos nombrs, con coma en urlPatterns.
initParams, valores iniciales, para poener un valor que va a ser constante, leer una vez... un directorio por ej donde va a guardar ficheros. o tipos validos, "jpg,gif,png"...  y peticiones 5, que luego ha hecho una declaracion global private int peticiones ( en el metodo init) . El init se ejecuta solo una vez. Se inicia con valor 5 y con cada una de las peticiones se valor aumentará. 

init hereda de HttpServlet, y todos sus metodos (en override....) ![[Pasted image 20240920174324.png]]
init, destroy, doget y dopost

usuario -> init -> service-> hilo/thread -> doPost/doGet   paralelos/concurrente (ampliar)

# 30/09/2024

Está subido al github
Por ahora monolíticas las apps, con el war. A nivel de comunicación o relación, es una app cliente/servidor basica normal. 

Fase II del ParamsFormServlet nos habíamos quedado. 

Propone en ParamsFormServlet2.java no hacer un arraylist de errores. Cuando se pinte la página html, se escriba debajo, queremos saber qué mensaje corresponde a cada campo. Utilizamos para ello map -> (hashmap)

Metodo re request,  req.setAttribute , podemos mandarle lo que queramos de complejo.

linea 140. pasar request y el response. 

cómo varia dinamicamente el jsp? proximo dia. la forma es la de la linea 140, getServletContext().getRequestDispatcher(path).forward(req,resp) 

En memoria 1 instancia del servlet (1 objeto) por cada petición, el contenedor crea 1 hilo para hacer la petición. 

Por rutina lanzar siempre SERVIDOR primero y luego cliente. 

# 02/10/2024

entender forma 2 de index2.jsp,

si hay un errorr, perdemos los datos, vamos a ver como lo preveemos. 

Tienes que hacer una aplicacion web, que aparezca un formulario, hacer tales cosas con los campos, escribir un fichero de log. REPASAR COLECCIONES Y FICHEROS. 

Ver qué es un scriplet, por que un metodo dentro de un metodo no. 
funcion con <%!  !%>

fase 3 con bootstrapt 

Fase 4: 
ver index4.jsp linea 32 el value=...

API Stream

Ver todos los materiales de apoyo.
 
Mañana unidad de trabajo 3

# 03/10/2024

Teoría cabeceras http

enumeration se recorre al estilo de iterator 

```java
Enumeration<String> headerNames = req.getHeaderNames();
   while (headerNames.hasMoreElements()) {
       String cabecera = headerNames.nextElement();
        out.println("<li>" + cabecera + ": " + req.getHeader(cabecera) + "</li>")
         }
```



servlers -> controllers 

listas/colecciones,  buscar, necesitas el metodo equals. 

Comparable y Comparator. 

# 04/10/2024

Manejar las cabeceras del response, no en html, en otro formato.


las propias: t . 17:04 
Cómo hacer clases genéricas.
- [ ] Mirar lo de las copias profundas/superficiales de arrays / listas
odo


metodo response para que pueda llevar tildes?

# 10/10/2024

Cabeceras : ## Ejercicio 5.3: buscar producto. En el caso de que no se encuentre devolveremos un Status HTTP Response 404

# 14/10/2024

podemos guardar cualquier cosa en sesiones basicamente.
accedemos a httpsession, siempre existe. 

getSession  con true por defecto?

# 18/10/2024
JDBC
executeQuery (select)
executeUpdate (insert, update, delete).

try with resources MEJOR , para close.

no vamos a hacer transacciones, con JPA sí
ver presentaciones jdbc


# 24/10/2024

teoria cdi, spring, quarkus.

# 09/01/2024
19:21 

# 20/01/2024
Validaciones quedaron pendientes

# 27/01/2024
mirar lo de n a m , la tabla intermedia si se actualiza al inscribir estudiantes.
