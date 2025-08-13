# SQL -> STRING FUNCTIONS
## SUMMARY
> [!summary]
> 
- - - 

## DEFINITION

**Official:**  
> Las funciones de cadenas permiten manipular y analizar valores de texto, como concatenar, buscar o transformar cadenas.

**Personal:**  
> Estas funciones te permiten trabajar con textos de manera flexible, desde cambiar mayúsculas hasta extraer palabras específicas.

---

## QUESTIONS

>[!tip]- **¿Qué hace CONCAT()?**  
> Combina dos cadenas en una sola.  
> ```sql
> SELECT CONCAT('Hola, ', 'Mundo') AS saludo FROM DUAL;
> ```

>[!question]- **¿Cómo convertir una cadena a mayúsculas o minúsculas?**  
> Usando `UPPER()` o `LOWER()`.  
> ```sql
> SELECT UPPER(nombre), LOWER(nombre) FROM empleados;
> ```

>[!warning]- **¿Qué hace SUBSTR()?**  
> Extrae una parte específica de una cadena según la posición inicial y la longitud.  
> ```sql
> SELECT SUBSTR(nombre, 1, 3) AS iniciales FROM empleados;
> ```

>[!danger]- **¿Cómo buscar la posición de un carácter en una cadena?**  
> Usando `INSTR()`.  
> ```sql
> SELECT INSTR('Hello, World', ',') AS posicion FROM DUAL; -- Resultado: 6
> ```

---

## USE

## C.1- **BÚSQUEDA Y TRANSFORMACIÓN:**
```sql
SELECT UPPER(nombre) AS nombre_mayus, 
       LOWER(nombre) AS nombre_minus, 
       LENGTH(nombre) AS longitud 
FROM empleados;
```

## C.2- **CONCATENACIÓN:**

```sql
SELECT CONCAT(nombre, ' ', apellido) AS nombre_completo 
FROM empleados;
```

