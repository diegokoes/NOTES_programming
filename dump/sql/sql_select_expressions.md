# SQL -> SELECT EXPRESSIONS
## DEFINITION

**Official:**  
> Las expresiones en `SELECT` permiten realizar cálculos, concatenaciones y transformaciones directamente en la consulta, devolviendo columnas personalizadas.

**Personal:**  
> Son combinaciones de operaciones y funciones aplicadas en el `SELECT` para manipular o transformar los datos recuperados.

---

## QUESTIONS

>[!tip]- **¿Qué es una expresión en SELECT?**  
> Es cualquier operación que genera un nuevo valor en la consulta.  
> ```sql
> SELECT salario * 1.10 AS salario_con_aumento 
> FROM empleados;
> ```

>[!question]- **¿Cómo se usa una expresión condicional en SELECT?**  
> Usando `CASE` para devolver valores basados en condiciones.  
> ```sql
> SELECT nombre, 
>        CASE 
>          WHEN salario > 50000 THEN 'Alto'
>          ELSE 'Bajo'
>        END AS rango_salarial 
> FROM empleados;
> ```

>[!danger]- **¿Qué hace COALESCE()?**  
> Devuelve el primer valor no nulo de una lista.  
> ```sql
> SELECT COALESCE(comision, 0) AS comision_real 
> FROM empleados;
> ```

---

## USE

## C.1- **CÁLCULOS Y TRANSFORMACIONES:**
```sql
SELECT nombre, salario * 1.20 AS salario_aumentado 
FROM empleados;
```

## C.2- **EXPRESIONES CONDICIONALES:**

```sql
SELECT nombre, 
       CASE 
         WHEN dept_no = 10 THEN 'Ventas'
         WHEN dept_no = 20 THEN 'Producción'
         ELSE 'Otros'
       END AS departamento 
FROM empleados;
```

## C.3- **CONCATENACIÓN DE CAMPOS:**
```sql
SELECT CONCAT(nombre, ' ', apellido) AS nombre_completo 
FROM empleados;
```