# JAVA -> STREAMS -> INTERMEDIATE OP.

## SUMMARY
> [!summary]-
> 
- - - 

## DEFINITION
**Official:**
> Las operaciones intermedias son las que procesan elementos de un Stream y devuelven otro Stream, permitiendo encadenar más operaciones.

**Personal:**
>Son como "filtros" o "transformadores" que aplicas a los datos mientras pasan por el Stream.
- - - 

## THEORY

### EXAMPLES

**1. `filter(Predicate)`**  
Filtra elementos según una condición.  
```java
List<Integer> numeros = List.of(1, 2, 3, 4);
List<Integer> pares = numeros.stream()
						     .filter(n -> n % 2 == 0)
						     .collect(Collectors.toList());
```

**2. `map(Function)`**  
Transforma cada elemento del Stream.
```java
List<String> nombres = List.of("Ana", "Carlos");
List<Integer> longitudes = nombres.stream()
								  .map(String::length)
							      .collect(Collectors.toList());
```

**3.  `sorted(Comparator)`**  
Ordena los elementos.
```java
List<String> nombres = List.of("Carlos", "Ana");
List<String> ordenados = nombres.stream()
							    .sorted()
							    .collect(Collectors.toList());
```
## QUESTIONS
>[!tip]- **¿Las operaciones intermedias se ejecutan inmediatamente?** 
No, las operaciones intermedias son **perezosas** y no se ejecutan hasta que se invoque una operación terminal.

> [!question]- **¿Qué diferencia hay entre `filter()` y `map()` en Streams?**
>- `filter()`: Selecciona elementos según una condición booleana.
>- `map()`: Transforma elementos del Stream aplicando una función.

> [!question]- **¿Qué hace el método `sorted()` en un Stream?**
Ordena los elementos del Stream. Por defecto, utiliza el orden natural o un `Comparator` si se especifica.

>[!warning]- **¿Cómo utilizarías `flatMap()` para transformar una lista de listas en una sola lista?**
 `flatMap()` toma cada lista dentro del Stream y las aplana en un único flujo continuo de elementos.

>[!danger]- **¿Cómo optimizarías el uso de operaciones intermedias para trabajar con grandes conjuntos de datos?**
Utilizando operaciones perezosas como `limit()` para reducir el tamaño del Stream antes de procesarlo por completo.

- - -
