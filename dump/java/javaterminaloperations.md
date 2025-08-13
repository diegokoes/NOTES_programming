# JAVA -> STREAM -> TERMINAL OPS.
## SUMMARY
> [!summary]-
> 
- - - 

## DEFINITION
**Official:**
> Las operaciones terminales son las que consumen el Stream, devolviendo un resultado o efecto secundario, como una lista o un entero.

**Personal:**
>Son "el final del flujo", donde obtienes el resultado procesado de los datos.
- - - 
## QUESTIONS
>[!warning]- **¿Cómo podrías combinar `reduce()` y `filter()` para sumar solo los números pares de un Stream?**
Primero usaría `filter()` para seleccionar los números pares y luego aplicaría `reduce(0, Integer::sum)` para sumarlos.

## EXAMPLES

**1. `collect(Collectors)`**  
Recoge los elementos del Stream en una colección.
```java 
List<String> nombres = Stream.of("Ana", "Diego").collect(Collectors.toList());
```

**2. `forEach(Consumer)`**  
Aplica una acción a cada elemento del Stream.
```java
List.of("Ana", "Diego").stream().forEach(System.out::println);
```
**3. `reduce(BinaryOperator)`**  
Combina elementos en un único resultado.
```java
int suma = Stream.of(1, 2, 3).reduce(0, Integer::sum);
```
- - - 
#java