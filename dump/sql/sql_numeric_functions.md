# SQL -> Numeric Functions
## Definition

**Official:**  
> Las funciones numéricas en SQL permiten realizar cálculos matemáticos, redondeos, operaciones con valores nulos, entre otros.

**Personal:**  
> Son herramientas que facilitan operaciones matemáticas en SQL, útiles para realizar cálculos complejos directamente en las consultas.

---

## Questions

>[!tip]- **¿Qué hace ABS()?**  
> Devuelve el valor absoluto de un número.  
> ```sql
> SELECT ABS(-5) AS absoluto FROM DUAL; -- Resultado: 5
> ```

>[!question]- **¿Cuál es la diferencia entre ROUND() y TRUNC()?**  
> - `ROUND()` redondea un número al entero o decimal más cercano.  
> - `TRUNC()` elimina los decimales sin redondear.  
> ```sql
> SELECT ROUND(2.666, 2), TRUNC(2.666, 2) FROM DUAL; -- Resultado: 2.67, 2.66
> ```

>[!warning]- **¿Qué hace NVL()?**  
> Sustituye los valores `NULL` por un valor especificado.  
> ```sql
> SELECT NVL(comision, 0) AS comision FROM empleados;
> ```

>[!danger]- **¿Cómo calcularías el promedio de un campo numérico?**  
> Usando `AVG()`, que calcula la media aritmética.  
> ```sql
> SELECT AVG(salario) AS promedio FROM empleados;
> ```

---

## Use

## C.1- **Operaciones Básicas:**
```sql
SELECT salario - 4000 AS ajuste, ABS(salario - 4000) AS absoluto 
FROM empleados;
```

## C.2- **Redondeo y Truncamiento:**
```sql
SELECT ROUND(2.6668888, 2) AS redondeo, TRUNC(2.6668888, 2) AS truncado 
FROM DUAL;
```

## C.3- **Manejo de Nulos con NVL:**
```sql
SELECT salario, NVL(comision, 0) AS comision_actualizada 
FROM empleados;
```

