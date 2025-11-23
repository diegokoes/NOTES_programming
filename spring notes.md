# SPRING NOTES
## SET UP
### DEPENDENCIES
- Lombok
- Spring Web
- Spring Security
- H2 Database
- Validation
- Spring Data JPA
- Spring Dev Tools
### POM
```xml
<dependency>  
    <groupId>org.mapstruct</groupId>  
    <artifactId>mapstruct</artifactId>  
    <version>1.5.5.Final</version>  
</dependency>  
<dependency>  
    <groupId>org.mapstruct</groupId>  
    <artifactId>mapstruct-processor</artifactId>  
    <version>1.5.5.Final</version>  
</dependency>
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-api</artifactId>  
    <version>0.12.6</version>  
</dependency>  
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-impl</artifactId>  
    <version>0.12.6</version>  
    <scope>compile</scope>  
</dependency>  
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-jackson</artifactId>  
    <version>0.12.6</version>  
    <scope>compile</scope>  
</dependency>
<!-- https://mvnrepository.com/artifact/me.paulschwarz/spring-dotenv -->
<dependency>
    <groupId>me.paulschwarz</groupId>
    <artifactId>spring-dotenv</artifactId>
    <version>4.0.0</version>
</dependency>
<dependency> 
	<groupId>org.thymeleaf.extras</groupId> 
	<artifactId>thymeleaf-extras-springsecurity6</artifactId> 
</dependency>
```

```xml
<path>
   <groupId>org.mapstruct</groupId>
   <artifactId>mapstruct-processor</artifactId>
   <version>1.5.5.Final</version>
</path>
``` 
## ENTITY 
Sus campos tienen que coincidir con los campos de una tabla. Con annotaciones:
```java
@Getter 
@Setter
@Entity 
@AllArgsConstructor /*-> Para que sea una Entidad tiene que tener también un constructor vacío*/
@NoArgsConstructor
public class 'Blablabla' => 'blablabla' tablename
o @TableName
//@Id <- clave primaria
//@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name="nombre")/*<-nombreTabla*/
private String firstName

@OneToMany(mappedBy ="fabricante", cascade = CascadeType.ALL, orphanRemoval = true)
// ONE TO MANY / MANY TO ONE
// En la parte de 1 -> Un objeto 
// En la parte de n -> Una lista
```


**Objetos bidireccionales**
Entidad propietaria y entidad inversa

mapped by -> inversa - 
//! TODO Ver lo de llamadas cíclicas ?? 
con el join -> entidad propietaria

//TODO ver extends GrantedAuthority


cuidado con lo de defer-datasource-initalization, tiene que ser true o lee primero data.sql y casca.
### VALIDATIONS
length, nullable, precision, scale...
## DTO 
@Getters
@Setters
@Data // lo hace todo
@Data =  @Get,Set... @RequiredArgs..., @EqualsAndHashCode,  @ToString
@AllArgsConstructor
@NoArgsConstructor
### VALIDATIONS
@DecimalMin( value = "100.00", message = " " )
@Size( min=4, max=4, message=" " )
@NotBlank

## CONTROLLER
@RestController  -> devuelve JSON
@RequestMapping("/auth") // para todos los endpoints hijos
> Inyectar Repository al controller

1. (Peor) Inyección por 
2. Inyección por constructor

Devolvemos ResponseEntity y no Entities, DTO's
siempre devolvemos un ResponseEntity (HttpResponse) que encapsula lo que se devuelve en el body (cualquier cosa).

@RequestBody-> viene un json. 
@RequestBody CustomerDTO customer -> que va a tener exactamente las mismas propiedades que CustomerDTO. Necesita que tenga 1 constructor vacío y setters.


@RequestParam ?

Lista de Entity a Lista de DTO -> .stream, .map return.... .toList()

ResponseEntity.notFound().build() (vacio)

ResponseEntity`<?>`

!! NO DOUBLE WRAP OPTIONAL 

@Valid @RequestBody en un controller, porque tienen el mismo nombre las de JSOn <-> DTO. 

@JsonProperty(access = JsonProperty.Access.WRITE_ONLY) 
^--- Sólo se usa propiedad al crear un producto, no al listar.

@Valid solo para DTO's 

@Pattern(regexp="^..$",message="") !!ANTES DE LA DECLARACION DE LA VARIABLE 
^para RequestParam/PathVariable


@Put modificar todo el objeto
@Patch -> parciales, no necesitas mandar todo. 
No sabemos qué vamos a recibir: Map de String, Object para el JSON en el ejemplo de ProductoApiRest. 
\\_ usamos **reflexión** 

relacion tabla tipo Producto que tiene objeto Fabricante:
objectMapper.converValue... 
!TODO ver pasos en el github
**¿Entra reflexión en el examen?**


? extends globalDTO? y //  \<?\>

### VALIDATIONS


## REPOSITORY

Interface que hereda de CrudRepository, la básica. 

> CrudRepository solo tiene Iterable... JPARepository da List , etc.... usaremos JPARepository

> ResponseEntity?  !TODO
siempre DTOS, entradas DTO's, contra la BD siempre ENTITIES

> Inyección por dos tipos:
> 1. por propiedad (autowired)
> 2. MEJOR por constructor (@RequiredArgsConstructor // crear un constructor con propiedades final)


Nunca devolvemos 1 entity.

si @AllArgsConstructor y es un @Entity tienes que crear un constructor vacío.  @NoArgsConstructorw

@Id -> PK 
@GeneratedValue -> strategy = GenerationType.Identity


## MAPPER

```java
package com.example.myapp.mapper;

import com.example.myapp.entity.Customer;
import com.example.myapp.model.CustomerDTO;
import org.mapstruct.Mapper;

@Mapper(componentModel = "spring")
public interface CustomerMapper {

    CustomerDTO toDto(Customer entity);

    Customer toEntity(CustomerDTO dto);
}
```
En el caso de nombres diferentes de propiedades entre el DTO y el Entity -> @Mapping (source = , target = )

Nombres iguales de propiedades DTO<->ENTIDAD **no hace falta hacer nada** 

Si tuvieran nombres distintos :  
```java
@Mapping(source="nombrePropiedadENTIDAD", target="nombrePropiedadDTO")  
//si es de DTO a ENTIDAD -
> se invierten  
@InheritInverseConfiguration lo hace solo
```

## GLOBALEXCEPTIONHANDLER
```java
@ControllerAdvice
public class GlobalExceptionHandler...
	@ExceptionHandler 

```
- Excepción personalizada: 
	- extends [RuntimeException | ... ]

en el service  `.orElseThrow()`


**@BUILDER** DE LOMBOK, sin el new. Para instancias que una vez creadas no se van a tocar, simplifica la creación.## SUMMARY

@Valid para validar JSON en el body del request. 

@Validated para

Próximo día: en vez de recibir un ProductoDTO, un Map.  en el PatchMapping.
¿Web Client?


## CONFIGURACION PERSONALIZADA 

importamos @Value de SpringFramework

https://github.com/diegokoes/DWES-03-2025-26/tree/main/EJERCICIOS  
^----- al final

@Configuration
@PropertySource...
@EnableConfigurationProperties...
Clase App config 

## SECURITY

`... implements UserDetails` - para gestionar permisos.

**Examen -> personalizar seguridad, para reglas de permisos y endopoints**

//TODO Ver ROLES DUPLICADOS 

>[!tip] @PreAuthorize y @PostAuthorize
>  Necesita @EnableMethodSecurity

>[!danger] EnableWebSecurity
> 
EnableWebSecurity no es necesario con Spring Boot 3.X / Security 6.x
## THYMELEAF

@Configuration
class en config de MvcConfig inmplements WebMvcConfigurer

registry.addViewControllers y dentro de él el mapeo de ruta y vistas 


@{/path} -> manera de resolver URL's 

siempre pasar por controller de vista a vista. Principal (autentificacion) //TODO *model* y *principal* ?? 

//TODO comunicación MVC > API REST

Par pasar de un controller a una vista, Spring proporciona Model model.  model.adAttribute.
ej. restaurants y le pasas una lista.

@Configuration
 AppConfig (en carpeta config): 
 @Value("${nombrepropiedadApplicationProperties}")

¿Web Client? -> en AppConfig (WebFlux) 

@RequiredArgsConstructor
|_ para inyectar Web Client en el **SERVICE** . 
|_ Hay 2 Beans mismo nombre? nombre diferente var.

Restaurants -> NO PAGING
Platos -> Paging

como si fuera un repository, con métodos en vez de BBDD con métodos de HTTP requests.

.get() -> .uri(sobre la base url) -> .retrieve() -> bodyToMono(clase dto) -> block()  ... 

En MVC; excepction handler devolvemos una página a la que ir.
le metemos un atributo y redireccionamos

**ETIQUETAS THYMELEAF** 

th:
...

**Utility Objects** 

para accedder a clases de utilidades


**MVCCONFIG SÓLO LAS PÁGINAS SIN LOGICA; ESTÁTICAS, SI HAY CONTROLLER PARA ESA RUTA, SE QUITA DEL MVC CONFIG** 

## EXAMEN

- Seguridad de endpoints en SecurityConfig, en los endpoints o como queramos.


@ModelAttribute -> para recoger datos de una vista 