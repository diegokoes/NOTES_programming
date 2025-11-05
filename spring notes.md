# SPRING NOTES

> DTO -> para no devolver el entity directamente 

Tantos servicios como entities en general, y tantas entities como tablas
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

## DTO 
No hay etiquetas DTO
`Necesita Getters y Setters`:
@Data =  @Get,Set... @RequiredArgs..., @EqualsAndHashCode,  @ToString
@AllArgsConstructor
@NoArgsConstructor

> **Dependencia:** `Validation`

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

## CONTROLLER 
@RestController  -> devuelve JSON
@RequestMapping("/auth") // para todos los endpoints hijos
> Inyectar Repository al controller
	
1. (Peor) Inyección por 
2. Inyección por constructor

Devolvemos ResponseEntity y no Entities, DTO's
siempre devolvemos un ResponseEntity (HttpResponse) que encapsula lo que se devuelve en el body (cualquier cosa).


@RequestBody-> viene un json
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

## Configuracion personalizada 

importamos @Value de SpringFramework

https://github.com/diegokoes/DWES-03-2025-26/tree/main/EJERCICIOS  
^----- al final

@Configuration
@PropertySource...
@EnableConfigurationProperties...
Clase App config 

## Security

`... implements UserDetails` - para gestionar permisos.