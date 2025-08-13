# SQL -> DATE FUNCTIONS
## DEFINITION

**Official:**  
> Las funciones de fecha en SQL permiten trabajar con valores temporales para calcular intervalos, extraer partes de fechas o realizar operaciones.

**Personal:**  
> Son esenciales para manejar fechas en SQL, desde comparar tiempos hasta calcular diferencias en días o meses.

---

## QUESTIONS

>[!tip]- **¿Qué hace SYSDATE?**  
> Devuelve la fecha y hora actuales del sistema.  
> ```sql
> SELECT SYSDATE AS fecha_actual FROM DUAL;
> ```

>[!question]- **¿Cómo extraer el año o mes de una fecha?**  
> Usando `EXTRACT()`.  
> ```sql
> SELECT EXTRACT(YEAR FROM fecha_nacimiento) AS anio 
> FROM empleados;
> ```

>[!warning]- **¿Qué hace ADD_MONTHS()?**  
> Suma o resta meses a una fecha.  
> ```sql
> SELECT ADD_MONTHS(SYSDATE, 3) AS fecha_futura FROM DUAL;
> ```

>[!danger]- **¿Cómo calcular la diferencia en días entre dos fechas?**  
> Restando las fechas directamente.  
> ```sql
> SELECT fecha_fin - fecha_inicio AS dias_diferencia 
> FROM proyectos;
> ```

---

## USE

## C.1- **FORMATO DE FECHAS:**
```sql
SELECT TO_CHAR(SYSDATE, 'DD/MM/YYYY') AS fecha_formateada 
FROM DUAL;
```

## C.2- **CÁLCULOS DE INTERVALOS:**
```sql
SELECT fecha_inicio, 
       fecha_inicio + 7 AS una_semana_despues 
FROM proyectos;
```