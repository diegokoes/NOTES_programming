# JAVA: STREAMS
## RELATED NOTES
1. [Optional](javaoptional.md)
2. [Intermediate Operations](javaintermediateoperations.md)
3. [Terminal Operations](javaterminaloperations.md)
## SUMMARY
> [!summary]
> 

## THEORY

![[372081412-349c5bb6-903c-422e-be19-63db2f66ff77.png]]

## QUESTIONS
	
- > [!tip]- **¿Qué es un Stream?**
    Es un flujo de datos que se puede procesar con operaciones funcionales como `filter()`, `map()` o `reduce()`.

- > [!tip]- **¿Qué diferencia hay entre Streams y colecciones?**
    -**Streams:** Son inmutables y procesan datos  
    de manera funcional.
    -**Colecciones:** Son mutables y almacenan los datos directamente.

- > [!question]- **¿Un Stream modifica la colección original?**
    No. Los Streams no alteran los datos originales; devuelven un nuevo flujo de datos.

- > [!warning]- **¿Cuáles son las operaciones intermedias en un Stream?**
    Las operaciones intermedias, como `filter()`, `map()` y `sorted()`, transforman un Stream en otro Stream. Para aprender más, consulta el archivo [[javaintermediateoperations]].

- > [!warning]- **¿Cuáles son las operaciones terminales en un Stream?**
    Las operaciones terminales, como `collect()`, `forEach()` y `reduce()`, consumen el Stream para generar un resultado. Para más detalles, revisa el archivo [[javaterminaloperations]].

## EXAMPLES

```java
List<String> nombres = Arrays.asList("Diego", "Ana", "Carlos");

// Filtrar nombres que comienzan con "A"
List<String> filtrados = nombres.stream()
    .filter(nombre -> nombre.startsWith("A"))
    .collect(Collectors.toList());

System.out.println(filtrados); // [Ana]
```

- - -
