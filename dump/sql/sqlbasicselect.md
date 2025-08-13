# SQL -> SELECT
## DEFINITION

**Official:**  
> El comando `SELECT` permite recuperar datos de una base de datos, definiendo columnas específicas o utilizando expresiones para transformarlos.

**Personal:**  
> `SELECT` es el punto de partida de casi todas las consultas en SQL, desde operaciones básicas hasta las más complejas.

---

## QUESTIONS

>[!tip]- **¿Qué hace el comando SELECT?**  
> Recupera datos de una tabla, permitiendo filtrar, transformar y organizar resultados.  
> ```sql
> SELECT nombre, edad FROM empleados;
> ```

>[!question]- **¿Qué hace DISTINCT en un SELECT?**  
> Elimina duplicados en los resultados.  
> ```sql
> SELECT DISTINCT departamento FROM empleados;
> ```

>[!danger]- **¿Qué diferencia hay entre WHERE y HAVING?**  
> - `WHERE`: Filtra datos antes de las agregaciones.  
> - `HAVING`: Filtra datos después de realizar las agregaciones.  
> ```sql
> SELECT departamento, AVG(salario) 
> FROM empleados
> GROUP BY departamento
> HAVING AVG(salario) > 50000;
> ```

---

## USE

## C.1- **EJEMPLO BÁSICO:**
```sql
SELECT nombre, salario FROM empleados;
```

## C.2- **USANDO FILTROS:**

```sql
SELECT nombre, salario 
FROM empleados 
WHERE salario BETWEEN 30000 AND 50000;
```

## C.3- **CON ALIAS:**

SELECT nombre AS "Empleado", salario AS "Sueldo" FROM empleados;