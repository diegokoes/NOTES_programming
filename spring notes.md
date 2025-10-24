
## SPRING NOTES

> DTO -> para no devolver el entity directamente 



> CrudRepository solo tiene Iterable... JPARepository da List , etc.... usaremos JPARepository

> ResponseEntity?  !TODO
siempre DTOS, entradas DTO's, contra la BD siempre ENTITIES


da error con RequestBody, el formulario manda como Params, no está enviando un JSON, por eso casca. 
¿por qué con 2 param no? 


> Inyección por dos tipos:
> 1. por propiedad (autowired)
> 2. MEJOR por constructor (@RequiredArgsConstructor // crear un constructor con propiedades final)

siempre devolvemos un ResponseEntity (HttpResponse) que encapsula lo que se devuelve en el body (cualquier cosa).

Nunca devolvemos 1 entity.

Tantos servicios como entities en general.

En el servicio -> 
