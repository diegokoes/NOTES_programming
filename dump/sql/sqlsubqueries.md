# SQL -> SUBQUERIES
## DEFINITION

**Official:**  
> Una subconsulta es una consulta dentro de otra consulta que devuelve datos intermedios utilizados por la consulta externa.

**Personal:**  
> Las subconsultas son como pequeñas preguntas que ayudan a responder la pregunta principal.

---

## QUESTIONS

>[!tip]- **¿Qué es una subconsulta?**  
> Es una consulta anidada dentro de otra consulta.  
> ```sql
> SELECT nombre 
> FROM empleados 
> WHERE dept_no = (SELECT dept_no FROM departamentos WHERE nombre = 'Ventas');
> ```

>[!question]- **¿Qué hace IN en una subconsulta?**  
> Verifica si un valor está en el conjunto devuelto por la subconsulta.  
> ```sql
> SELECT nombre 
> FROM empleados 
> WHERE dept_no IN (SELECT dept_no FROM departamentos WHERE ciudad = 'Madrid');
> ```

>[!danger]- **¿Qué diferencia hay entre subconsultas correlacionadas y no correlacionadas?**  
> - **No correlacionadas:** No dependen de la consulta externa.  
> - **Correlacionadas:** Dependen de valores de la consulta externa.  
> ```sql
> SELECT nombre 
> FROM empleados e 
> WHERE salario > (SELECT AVG(salario) FROM empleados WHERE dept_no = e.dept_no);
> ```

---

## USE

## C.1- **SUBCONSULTA BÁSICA:**
```sql
SELECT nombre 
FROM empleados 
WHERE salario > (SELECT AVG(salario) FROM empleados);
```

## C.2- **SUBCONSULTA CON IN:**

```sql
SELECT nombre 
FROM empleados 
WHERE dept_no IN (SELECT dept_no FROM departamentos WHERE ciudad = 'Madrid');
```

