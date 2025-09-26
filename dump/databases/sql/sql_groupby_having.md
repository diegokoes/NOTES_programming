
# SQL: GROUP BY & HAVING
## DEFINITION

**Official:**  
> `GROUP BY` agrupa filas con valores comunes en columnas específicas, permitiendo realizar cálculos agregados. `HAVING` filtra los resultados después de las agregaciones.

**Personal:**  
> `GROUP BY` organiza filas similares juntas y permite aplicar funciones como `SUM()` o `AVG()`. `HAVING` ayuda a filtrar estos grupos.

---

## QUESTIONS

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

## USE

## C.1- **EJEMPLO BÁSICO:**
```sql
SELECT departamento, COUNT(*) AS empleados_por_departamento 
FROM empleados 
GROUP BY departamento;
```

## C.2- **CON FILTROS:**
```sql
SELECT departamento, SUM(salario) AS total_salarios 
FROM empleados 
GROUP BY departamento 
HAVING SUM(salario) > 100000;
```