# SQL -> JOINS
## DEFINITION

**Official:**  
> Un `JOIN` combina filas de dos o más tablas basándose en una condición relacionada, como claves primarias y foráneas.

**Personal:**  
> `JOIN` es como un puente entre tablas. Une datos relacionados para trabajar con información más completa.

---

## QUESTIONS

>[!tip]- **¿Qué es un INNER JOIN?**  
> Recupera las filas que tienen coincidencias en ambas tablas.  
> ```sql
> SELECT empleados.nombre, departamentos.nombre 
> FROM empleados 
> INNER JOIN departamentos ON empleados.dept_no = departamentos.dept_no;
> ```

>[!question]- **¿Qué diferencia hay entre LEFT JOIN y RIGHT JOIN?**  
> - `LEFT JOIN`: Incluye todas las filas de la tabla izquierda y las coincidencias de la derecha.  
> - `RIGHT JOIN`: Lo contrario.  
> ```sql
> SELECT empleados.nombre, departamentos.nombre 
> FROM empleados 
> LEFT JOIN departamentos ON empleados.dept_no = departamentos.dept_no;
> ```

>[!danger]- **¿Qué es un FULL JOIN?**  
> Combina todas las filas de ambas tablas, incluso las que no tienen coincidencias.  
> ```sql
> SELECT empleados.nombre, departamentos.nombre 
> FROM empleados 
> FULL JOIN departamentos ON empleados.dept_no = departamentos.dept_no;
> ```

---

## USE

## C.1- **NATURAL JOIN:**
```sql
SELECT * 
FROM empleados 
NATURAL JOIN departamentos;
```

## C.2- **JOIN CON ALIAS:**
```sql
SELECT e.nombre, d.nombre AS "Departamento" 
FROM empleados e 
INNER JOIN departamentos d ON e.dept_no = d.dept_no;
```

