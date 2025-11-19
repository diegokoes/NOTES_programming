# SERVIDOR
## CARPETAS
- DTO
- entity
- service
- repository
- controller
- security
- mapper
- exception
## ¿QUÉ APLICACIÓN ESTAMOS MONTANDO?
1. API REST ? 
2. MVC ?

### API REST
- haces una petición a un endpoint  (API online) , via get, post, etc.  REST? -> devuelve **SIEMPRE** objeto JSON.
- puede ser una petición a un recurso ajeno, o a una aplicación que hemos hecho nosotros, en localhost:port...
### MVC                                                                                                            
- Modelo Vista Controllador. Que añade simplemente pues vista HTML. Y llama a una **API REST** 
- No sólo tienes HTML, metes autentificación de log in, controllers para pasar información a las vistas (HTML).

> Pueden interacturar entre ellas. En el examen seguramente nos de las dos.

- - - 
En una API Rest hacemos operaciones contra la Base de Datos. Selects, updates, deletes, etc.
- Los **ENTITY** son la representación en Java, de la estructura de las tablas en la base de datos

Cuando hacemos una petición a la API, **NUNCA DEVOLVEMOS ENTITY**.  -> **DEVOLVEMOS DTO** 
- Los **DTO**: En un DTO estableces la estructura, propiedades, etc del objeto JSON que los endpoints del controller van a devolver.

**Ciudadano\a** : ELIGES LO QUE DEVOLVER
entity : nombre, apellidos, edad, dni.       dto : nombreCompleto: (nombre+apellidos), edad.

***CONTROLLER + SERVICE + REPOSITORY***

- Repository: `extends JpaRepository<(Entity/Objeto), Long>`
	- Darte funcionalidad para hacer operaciones contra la base de datos, 
		suele estar vacía. Te da herramientas/facilidades para crear METODOS para hacer queries a BBDD
	|
	|
¿Dónde metemos la lógica?
	|
	|
-  Service: save(), update(), find() , findAll(), delete()... todo lo que quieras hacer sobre la base de datos. 
	Cuando hacer una operación como por ejemplo, borrar un producto por un ID que te han dado.
	Toda la lógica de negocio/código
	**¿Quién te lo da?**  **¿Por dónde viene?**
	|
	|
- Controller:     
	- Controlas si viene información a una URL, por POST, GET, PUT, DELETE, PATCH (Métodos HTTP)
						URL  +  Método HTTP
						@GetMapping("picante/recetas") 
		 PONE ALGUIEN ESA URL Y VA POR GET -> NOSOTROS CONTROLAMOS QUE DEVOLVEMOS
							**¡¡ PERO !!** 
				No metemos código tocho en el Controller, eso en el *SERVICE* 
				En el controller llamas a los métodos del *SERVICE* 
			

		EN EL CONTROLLER RECIBES LA URL, PERO TAMBIEN PUEDES RECIBIR POST POST UN OBJETO
				de un formulario, viene una nueva receta picante, y quieres añadirla a la base de datos.
	¿Qué necesitas? -> RecetaRepository -> RecetaService (con un insert) -> end point para recibir el objeto que quieres insertar, y que llame al método de RecetaService.

**Mapper** ¿QUÉ COJONES HACE MAPPER? 
					CONVIERTE: DTO < -- > ENTITY
						OK PERO PARA QUÉ
							|
							|
				Presentar/devolver información = DTO 
				Operaciones con BBDD = ENTITY


**Security**:
 - Es la configuración de seguridad de la aplicación Spring. 
			 **NO SIEMPRE LA MISMA** ----- **CAMBIA SI ESTÁS EN UNA REST API, O MVC** 
			 Puedes tener usuarios y roles (MVC) , JWT (RestAPI)... 
			 Y mapeas 


**Exception:** 
- GlobalExceptionHandler: @ExceptionHandler para cada excepción que queramos tratar, personalizadas incluidas. 
			  Y podemos trabajar con un ErrorDTO para luego retornar un ResponseEntity
			  ¿Qué hace el Response Entity?
			   |_ manejamos qué codigo HTTP de error queremos devolver
			   |_ le metemos un body con ErrorDTO (va a ser diferente en cada exception).