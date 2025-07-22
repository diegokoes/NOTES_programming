# SQL -> CONNECT BY
## Definition

**Official:**  
> `CONNECT BY` se utiliza en SQL para construir consultas jerárquicas, como relaciones de empleados y supervisores, o estructuras de árbol.

**Personal:**  
> Es una característica que permite navegar por relaciones jerárquicas en una tabla, útil para analizar estructuras como árboles o grafos.

---

## Questions

>[!tip]- **¿Qué hace CONNECT BY en SQL?**  
> Crea una relación jerárquica entre filas de una tabla, como padre-hijo.  
> ```sql
> SELECT empleado_id, supervisor_id 
> FROM empleados 
> CONNECT BY PRIOR empleado_id = supervisor_id;
> ```

>[!question]- **¿Qué es START WITH en consultas jerárquicas?**  
> Define el punto de inicio de la jerarquía.  
> ```sql
> SELECT empleado_id, supervisor_id 
> FROM empleados 
> START WITH supervisor_id IS NULL 
> CONNECT BY PRIOR empleado_id = supervisor_id;
> ```

>[!danger]- **¿Cómo limitar la profundidad de una jerarquía?**  
> Usando `LEVEL`, que representa el nivel de profundidad en la jerarquía.  
> ```sql
> SELECT empleado_id, supervisor_id, LEVEL 
> FROM empleados 
> CONNECT BY PRIOR empleado_id = supervisor_id 
> AND LEVEL <= 3;
> ```

---

## Use

## C.1- **Construir una Jerarquía:**
```sql
SELECT empleado_id, supervisor_id, LEVEL 
FROM empleados 
START WITH supervisor_id IS NULL 
CONNECT BY PRIOR empleado_id = supervisor_id;
```

## C.2- **Ordenar Jerarquías:**
```sql
SELECT empleado_id, supervisor_id, LEVEL 
FROM empleados 
START WITH supervisor_id IS NULL 
CONNECT BY PRIOR empleado_id = supervisor_id 
ORDER SIBLINGS BY empleado_id;
```

## C.3- **Contar Niveles:**
```sql
SELECT MAX(LEVEL) AS profundidad_maxima 
FROM empleados 
CONNECT BY PRIOR empleado_id = supervisor_id;
```

