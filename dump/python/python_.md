Debes crear un programa en Java que simule el lanzamiento de dados y realice algunos análisis de los resultados. El programa debe ser ejecutado desde la línea de comandos (consola , nada de interfaz gráfica)

1. El programa debe preguntar al usuario cuántos dados desea lanzar (entre 1 y 8).,
2. Cada dado debe generar un número aleatorio entre 1 y 6.,
3. Los resultados deben mostrarse asociados a letras comenzando por la A. Por ejemplo: A: 3, B: 5, C: 1, etc.,
4. Después de mostrar los resultados, el programa debe presentar un análisis que incluya:
    
    - La suma total de los dados,
    - El valor promedio (con dos decimales),
    - Cuántos dados mostraron números pares e impares,
    - El valor más alto y el más bajo obtenidos,
    
    ,
5. El programa debe detectar y anunciar configuraciones especiales:
    
    - "Escalera" si los números están en secuencia (por ejemplo: 1,2,3,4),
    - "Todos iguales" si todos los dados muestran el mismo valor,
    - "Mayoría" si más de la mitad de los dados muestran el mismo valor,
    
    ,
6. Implementa un sistema de puntuación según estas reglas:
    
    - Cada dado par suma su valor a la puntuación,
    - Cada dado impar resta 1 punto,
    - Si se obtiene una "Escalera", se duplica la puntuación final,
    - Si todos los dados son iguales, se triplica la puntuación final,
    
    ,
7. Ofrece al usuario la opción de volver a tirar los dados o finalizar el programa.


- - -
#python