
# SQL: GROUP BY & HAVING
## Definition

**Official:**  
> `GROUP BY` agrupa filas con valores comunes en columnas específicas, permitiendo realizar cálculos agregados. `HAVING` filtra los resultados después de las agregaciones.

**Personal:**  
> `GROUP BY` organiza filas similares juntas y permite aplicar funciones como `SUM()` o `AVG()`. `HAVING` ayuda a filtrar estos grupos.

---

## Questions

>[!tip]- **¿Qué hace GROUP BY en SQL?**  
> Agrupa filas con valores comunes.  
> ```sql
> SELECT dept_no, AVG(salario) 
> FROM empleados 
> GROUP BY dept_no;
> ```

>[!question]- **¿Cómo se combina HAVING con GROUP BY?**  
> `HAVING` filtra los grupos creados por `GROUP BY`.  
> ```sql
> SELECT dept_no, AVG(salario) 
> FROM empleados 
> GROUP BY dept_no 
> HAVING AVG(salario) > 50000;
> ```

>[!danger]- **¿Es obligatorio usar GROUP BY con funciones agregadas?**  
> Sí, cuando una consulta mezcla funciones agregadas con columnas no agregadas.  
> ```sql
> SELECT dept_no, COUNT(*) 
> FROM empleados 
> GROUP BY dept_no;
> ```

---

## Use

## C.1- **Ejemplo Básico:**
```sql
SELECT departamento, COUNT(*) AS empleados_por_departamento 
FROM empleados 
GROUP BY departamento;
```

## C.2- **Con Filtros:**
```sql
SELECT departamento, SUM(salario) AS total_salarios 
FROM empleados 
GROUP BY departamento 
HAVING SUM(salario) > 100000;
```