## SPRING NOTES

> DTO -> para no devolver el entity directamente 

Tantos servicios como entities en general, y tantas entities como tablas
### Entity 
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

### DTO 
No hay etiquetas DTO
`Necesita Getters y Setters`:
@Data =  @Get,Set... @RequiredArgs..., @EqualsAndHashCode,  @ToString
@AllArgsConstructor
@NoArgsConstructor
### Repository

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

### Controller 
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
### Mapper

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

### X

