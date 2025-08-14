# SQL -> SETS
## SUMMARY
> [!summary]-
> 
- - - 

## DEFINITION

**Official:**  
> Las operaciones de conjuntos en SQL combinan resultados de múltiples consultas usando operadores como `UNION`, `INTERSECT` o `EXCEPT`.

**Personal:**  
> Son formas de combinar o comparar resultados de diferentes consultas, devolviendo conjuntos de datos únicos o comunes.

---

## QUESTIONS

>[!tip]- **¿Qué hace UNION en SQL?**  
> Combina los resultados de dos consultas eliminando duplicados.  
> ```sql
> SELECT nombre FROM empleados 
> UNION 
> SELECT nombre FROM clientes;
> ```

>[!question]- **¿Qué hace UNION ALL?**  
> Combina resultados sin eliminar duplicados.  
> ```sql
> SELECT nombre FROM empleados 
> UNION ALL 
> SELECT nombre FROM clientes;
> ```

>[!warning]- **¿Qué hace INTERSECT?**  
> Devuelve filas comunes entre dos consultas.  
> ```sql
> SELECT nombre FROM empleados 
> INTERSECT 
> SELECT nombre FROM clientes;
> ```

>[!danger]- **¿Qué hace EXCEPT?**  
> Devuelve filas que están en la primera consulta pero no en la segunda.  
> ```sql
> SELECT nombre FROM empleados 
> EXCEPT 
> SELECT nombre FROM clientes;
> ```

---

## USE

## C.1- **COMBINACIÓN BÁSICA:**
```sql
SELECT nombre FROM empleados 
UNION 
SELECT nombre FROM clientes;
```
## C.2- **CONSERVAR DUPLICADOS:**
```sql
SELECT nombre FROM empleados 
UNION ALL 
SELECT nombre FROM clientes;
```

## C.3- **FILTRAR COINCIDENCIAS:**
```sql
SELECT nombre FROM empleados 
INTERSECT 
SELECT nombre FROM clientes;
```
## C.4- **EXCLUIR RESULTADOS:**
```sql
SELECT nombre FROM empleados 
EXCEPT 
SELECT nombre FROM clientes;
```
- - - 
