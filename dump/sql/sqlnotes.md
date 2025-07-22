# A- ÍNDICE 
- [APUNTES](#APUNTES)
	-  [CONSULTAS](#CONSULTAS)
	- [JOIN](#JOIN)
	- [SUBCONSULTAS](#SUBCONSULTAS)
	- [FUNCIONES_NUMÉRICAS](#FUNCIONES_NUMÉRICAS)
	- [FUNCIONES_STRING](#FUNCIONES-STRING)
	- [FUNCIONES_FECHA](#FUNCIONES_FECHA)
	-  [GROUP BY Y HAVING](#GROUP-BY_HAVING)
	- [CONNECT-BY](#CONNECT-BY)
	- [SELECT-EXPRESSION](#SELECT-EXPRESSION)
	- [CONJUNTOS](#CONJUNTOS)
	- [INSERT/DELETE](#INSERT/DELETE)
- [EJERCICIOS](#EJERCICIOS)
	- [EJ1_CONSULTAS](#EJ1_CONSULTAS)
	- [EJ2_FUNCIONES](#EJ2_FUNCIONES)
# B- APUNTES
## B.1- CONSULTAS

Aliases no funcionan con WHERE
IN (OPCION1,OPCION2, ... )  -- Es como un OR,
BETWEEN 1000 AND 3000 o  NOT BETWEEN 7
LIKE 'A%' o NOT LIKE
DISTINCT 

## B.2- JOIN 
  -- Cuando la foreign key se llama igual que la pk -> NATURAL JOIN
	INNER JOIN BUSCAMOS LA CORRESPONDENCIA ENTRE LOS DOS, NO VAN A SALIR LOS QUE NO TIENEN PRESENCIA EN LA OTRA 
	LEFT JOIN o RIGHT JOIN 
	FULL JOIN
## B.3- SUBCONSULTAS
	IN PARA SUBCONSULTAS QUE DEVUELVEN VARIAS TUPLAS 
## B.4- FUNCIONES_NUMÉRICAS


    SELECT SALARIO - 400000, ABS(SALARIO-400000) FROM EMPLE; -- ABS VALOR ABSOLUTO 

    SELECT 13/2, MOD(5,2) AS RESTO FROM DUAL; -- AQUI PUEDE TENER DECIMALES, EN JAVA TIENE QUE SER ENTEROS - DECIMALES CON '.'
    SELECT FLOOR(3.999999999), CEIL(3.999999999) FROM DUAL;
    
    -- CUANDO UN VALOR ES NULL LO SUSTITUYE POR OTRO VALOR (NVL)
    SELECT SALARIO,COMISION FROM EMPLE;
    SELECT SALARIO,COMISION,NVL(COMISION,0) FROM EMPLE; -- CUANDO ES DISTINTA DE NULL, NO HACE NADA, SI ES NULL LO SUSTITUYE POR EL SEGUNDO CAMPO
    SELECT SALARIO,COMISION, NVL(SALARIO,0)+NVL(COMISION,0) AS "TTOTAL BIEN" FROM EMPLE;
    
    
    SELECT NVL(NOMBRE,'VACIO') 
           FROM NOMBRES;
       
    SELECT 2.6668888, ROUND(2.6668888,4) FROM DUAL;
    SELECT 2.6668888, TRUNC(2.6668888,4) FROM DUAL;                               -- TRUNC NO REDONDEA COMO ROUND
        
    SELECT NOMBRE_ALUMNO,NOTA1,NOTA2,NOTA3, ROUND((NVL(NOTA1,0)+NVL(NOTA2,0)+NVL(NOTA3,0))/3,2) AS "NOTA MEDIA" FROM NOTAS_ALUMNOS;
    SELECT POWER(2,4) FROM DUAL; -- POTENCIAS
    -- CALCULAR MEDIA . FUNCIONES DE GRUPO (AVG,COUNT,MAX,MIN,SUM,VARIANCE)
    SELECT ROUND(AVG(SALARIO),2) FROM EMPLE;
    SELECT COUNT(*) FROM EMPLE;
    
    SELECT COUNT(*) FROM EMPLE WHERE OFICIO='VENDEDOR';
    
    SELECT COUNT(*) FROM EMPLE WHERE COMISION IS NOT NULL;
    SELECT * FROM EMPLE;
    -- O 
    SELECT COUNT(COMISION) FROM EMPLE; -- CUENTA DIRECTAMENTE CUANDO HAYA DATOS
    
    SELECT COUNT(*) FROM EMPLE WHERE COMISION IS NULL;
    SELECT COUNT(*) - COUNT(COMISION) AS "SIN COMISION"
    FROM EMPLE;
    
    SELECT COUNT(DISTINCT OFICIO) FROM EMPLE; -- NUMEROS DE OFICIOS DIFERENTES 
    
    SELECT COUNT(DISTINCT DEPT_NO) FROM EMPLE;
    
    SELECT APELLIDO,SALARIO 
           FROM EMPLE
                WHERE SALARIO = (SELECT MAX(SALARIO)
                 FROM EMPLE);
                 
    -- EL QUE MAS GANA DEL DEPARTAMENTO 10
    
    SELECT APELLIDO, SALARIO, DEPT_NO
          FROM EMPLE
          WHERE SALARIO = (SELECT MAX(SALARIO)
                           FROM EMPLE
                           WHERE DEPT_NO=10)
                AND DEPT_NO=10; -- importante , si no, puede salir otro que no sea del dept-no =10
                
     -- donde trabaja el que mas gana
     
     select * from depart;
     
     SELECT LOC 
            FROM DEPART
               WHERE DEPT_NO = (SELECT DEPT_NO
                                FROM EMPLE 
                                WHERE SALARIO = (SELECT MAX(SALARIO) 
                                                 FROM EMPLE));
       -- APELLIDO, DNOMBRE DEL QUE MAS GANA                              
     SELECT E.APELLIDO,D.DNOMBRE
            FROM EMPLE E
                 INNER JOIN DEPART D
                  ON D.DEPT_NO=E.DEPT_NO
                   WHERE E.SALARIO = (SELECT MAX(SALARIO) FROM EMPLE);
      
     -- CUANTOS TRABAJADORES GANAN MAS QUE LA MEDIA DE LA EMPRESA
     (3.999999999
     
     SELECT COUNT(*) 
      FROM EMPLE 
       WHERE SALARIO > (SELECT AVG(SALARIO) FROM EMPLE);
                                                 
   -- LOCALIDAD DEL TRABAJADOR QUE GANA MAS DEL DEPT_NO 30   
   SELECT LOC 
          FROM DEPART
           WHERE DEPT_NO IN (SELECT DEPT_NO
                             FROM EMPLE 
                                  WHERE SALARIO = (SELECT MAX(SALARIO)
                                                   FROM EMPLE
                                                   WHERE DEPT_NO=30)
                                   AND 
                                   DEPT_NO=30); 
                                   
   
   -- Apellido, localidad de los que ganan mas de la media o menos de la mitad de la media de los salarios
   
   SELECT APELLIDO,LOC
          FROM EMPLE E
           NATURAL JOIN DEPART D
            WHERE E.SALARIO > (SELECT AVG(SALARIO)*2 -- LAS MEDIAS EXCLUYEN A LOS NULOS, NO DA ERROR
                               FROM EMPLE)           -- TODAS LAS FUNCIONES DE GRUPO EXCLUYEN A LOS NULOS
                  OR E.SALARIO<(SELECT (AVG(SALARIO))/2 
                                FROM EMPLE); 
   
   -- LEAST Y GREATEST               NO EXCLUYE LOS NULLS - FUNCIONA CON TUPLAS, NO ATRIBUTOS
   SELECT * FROM NOTAS_ALUMNOS;
   
   SELECT GREATEST(NVL(NOTA1,0),NVL(NOTA2,0),NVL(NOTA3,0))AS "NOTA MAS ALTA",NOMBRE_ALUMNO
          FROM NOTAS_ALUMNOS;
   SELECT LEAST(NVL(NOTA1,0),NVL(NOTA2,0),NVL(NOTA3,0))AS "NOTA MAS ALTA",NOMBRE_ALUMNO
          FROM NOTAS_ALUMNOS;
  
## B.5- FUNCIONES_STRING

-- ************* FUNCIONES DE CADENA VARCHAR2 *****************
   
   SELECT CHR(97) FROM DUAL; -- CODIGO ASCII
   
   SELECT ASCII('a') FROM DUAL; -- Al reves
   
   SELECT CONCAT('Tu apellido es: ', APELLIDO) -- DOS PARAMETROS PARA CONCATENAR, no mas
          FROM EMPLE;
                                            
   SELECT ('Tu apellido es: ' || APELLIDO) -- Otra manera con barra barra , varias concatenaciones, no solo 2
          FROM EMPLE;
          
   SELECT (APELLIDO || ' es ' || OFICIO)
          from emple;
   SELECT CONCAT(CONCAT(APELLIDO,' es '),OFICIO) -- Para solventar la limitacion de 2 parametros en concat, puedes hacer un concat de concat
          FROM EMPLE;
   
   SELECT LOWER(APELLIDO)
          FROM EMPLE;
   
   -- LPAD Y RPAD   NO ES IMPORTANTE
   
   SELECT LPAD(CADENA,TAMANIO,[CARACTER INSERCION]); -- sintaxis
   
   SELECT APELLIDO,LPAD(APELLIDO,20,'#')
          FROM EMPLE;
   SELECT APELLIDO,LPAD(APELLIDO,20,'ow')
          FROM EMPLE;
          
   -- LTRIM y RTRIM   SI SON MAS IMPORTANTES
   LTRIM(CADENA,[CARACTER A ELIMINAR]); -- VALOR POR DEFECTO LOS ESPACIOS EN BLANCO 
   
   SELECT 'PRUEBA PARA VER SI ELIMINA CARACTERES*****************' AS boop, RTRIM('PRUEBA PARA VER SI ELIMINA ESPACIOS**********','*' ) as owo
          FROM DUAL;
   -- REPLACE 
   
   SELECT REPLACE('BLANCO Y NEGRO','O','AS') FROM DUAL; -- LAS O'S POR AS   SI NO SE USA EL TERCER PARAMETRO, BORRA EL SEGUNDO , TERCER OPCIONAL
   
   -- TRANSLATE 
   SELECT REPLACE('SQLPLUS','SQL',123),              -- BUSCA LA CADENA ENTERA, LA CAMBIA INTEGRAMENTE POR 123
          TRANSLATE('SQLPLUS','SQL',123) FROM DUAL;  -- MISMA CADENA, NO BUSCA LA CADENA, BUSCA CADA CHAR, Y LOS REEMPLAZA, EN ESE ORDEN
                                                     -- EL TRANSLATE DA 123P3U1 , SIN NO PONES 3, BORRA EL TERCER CARACTER, EN ESTE CASO 'L' y no muestra 3
   -- SUBSTR
   
   SELECT SUBSTR('PROBANDO EL SUBSTRING',2,10) FROM DUAL; -- SI NO SE ESPECIFICA EL TERCER (NUM DE CARACTERES) LO HACE HASTA EL FINAL
                                                          -- DA 'ROBANDO EL'
   -- LENGTH
   -- LONGITUD DE LA CADENA
   SELECT LENGTH('PROBANDO EL LENGTH') FROM DUAL;
   
   SELECT APELLIDO,LENGTH(APELLIDO)
          FROM EMPLE;
   
   SELECT * FROM LIBRERIA;
   
   SELECT TEMA, LENGTH(RTRIM(TEMA)) -- TEMA ES CHAR15, ES LONGITUD FIJA, POR LO QUE ESPACIOS EN BLANCO, HAY QUE HACER RTRIM, VARCHAR2 ES VARIABLE, PEPE ES 'PEPE'
          FROM LIBRERIA;
   
   
   
-- 30/10/23

  -- INSTR (CADENA,CADENA_BUSQUEDA[,POSI_INICIO][,NUM_OCURRENCIA];
   
   SELECT INSTR('VUELTA CICLISTA A TALAMANCA','TA',1,1) FROM DUAL;
   SELECT INSTR('VUELTA CICLISTA A TALAMANCA','TA',6,2) FROM DUAL;

   
   SELECT * FROM LIBRERIA;
   
   SELECT INSTR(TEMA,' ')-1 FROM LIBRERIA; -- OTRA MANERA DE HACERLO COMO HICIMOS CON EL RTRIM, ENCONTRANDO PRIMER ESPACIO EN BLANCO Y -1

## B.6- FUNCIONES_FECHA


   SELECT SYSDATE FROM DUAL; -- FECHA DEL SISTEMA (TIMESTAMP = FECHA + HH--MM--SS)
   
   SELECT SYSDATE +1 FROM DUAL; -- CONVERSION AUTOMATICA, FUNCION DE FECHA
   SELECT '30/10/2023' +1 FROM DUAL; -- CASCA, NO TIENE CONVERSION AUTOMATICA, ES UNA CADENA, NO HAY FUNCION DE FECHA
   -- EN ESOS CASOS , CONVERSION EXPLICITA
   SELECT TO_DATE('30/10/2023') + 1 FROM DUAL;    -- El to_date tiene (cadenaque pasaar a fecha, y [formato])
   
   SELECT TO_DATE('30---10---23') +1 FROM DUAL; --CASCA
   SELECT TO_DATE('30---10---23','DD---MM---YY')+1 FROM DUAL; -- FUNCIONA, NECESITA EL FORMATO
   
   -- ASPECTO A UNA FECHA CON TO_CHAR (CONVIERTE ALGO A CADENA, Y PARA PASAR UNA FECHA CON UN FORMATO)
   SELECT TO_CHAR(6)|| TO_CHAR(5) FROM DUAL;
   
   SELECT APELLIDO,FECHA_ALT, TO_CHAR(FECHA_ALT,'DD/MONTH/YY') -- FORMATEANDO LAS FECHAS
          FROM EMPLE;
          
   SELECT TO_CHAR(TO_DATE('29/02/2024'),'Day,  dd " de " Month " del " yyyy') from dual; -- 'DDD' da el numero de dia dentro del año
   
   -- WW la semana del anio
   -- w la semana dentro del mes , d para el dia de la semana lunes(1)
   
   SELECT TO_CHAR(sysdate,'hh24:mi:ss') from dual;
   
   -- SELECT EXTRACT(DAY FROM FECHA)
   --                MONTH
   --                YEAR
   
   SELECT APELLIDO, FECHA_ALT, EXTRACT (MONTH FROM FECHA_ALT) FROM EMPLE;
   
   SELECT TRUNC(SYSDATE) + 9/24 FROM DUAL; -- SE SUELE USAR EL TRUNCATE PORQUE LO DEJA EN 0:00:00
   
   -- mes medio de alta en la empresa
   SELECT TRUNC(AVG(EXTRACT(MONTH FROM FECHA_ALT))) FROM EMPLE;
   
   -- Quienes se dio de alta en el mes medio de alta en la empresa + 2
   
   SELECT APELLIDO, extract ( MONTH FROM FECHA_ALT) AS MES, TO_CHAR(FECHA_ALT,'Month') as OTRO_MES -- CON VARIAS FORMAS
          FROM EMPLE
               WHERE EXTRACT(MONTH FROM FECHA_ALT) = (SELECT TRUNC(AVG(EXTRACT(MONTH FROM FECHA_ALT))+2) FROM EMPLE);
   -- Nombre de los departamentos que posean empleados que se dieran de alta 4 meses despues del mes medio de alta
   
   SELECT dnombre
          FROM depart
               WHERE DEPT_NO IN (SELECT DEPT_NO 
                               FROM EMPLE 
                               WHERE EXTRACT(MONTH FROM FECHA_ALT)
                              =              (SELECT 
                                              TRUNC(AVG(EXTRACT(MONTH FROM FECHA_ALT))+2) 
                                              FROM EMPLE));
    -- Apellidos y localidades de los que se dieron de alta un dia 23 de cualquier mes y cualquier anio
    
    SELECT APELLIDO, LOC, FECHA_ALT
           FROM EMPLE E
                NATURAL JOIN DEPART D
                 WHERE EXTRACT(DAY FROM FECHA_ALT) = 23;
                 
     -- APELLIDO Y LOCALIDADES DE LOS QUE SE DIERON DE ALTA EL MISMO ANIO QUE REY 
     
     SELECT apellido, loc
            from emple e
             natural join depart d 
                     where EXTRACT(YEAR FROM FECHA_ALT) = (SELECT EXTRACT(YEAR FROM  FECHA_ALT )
                                        FROM EMPLE
                                        WHERE APELLIDO='REY')
                           AND APELLIDO<>'REY';
      -- Apellido, localidad, nombre del mes de los que se dieron de alta el mismo mes de alta que el que gana menos de la empresa 
      
      SELECT APELLIDO, TO_CHAR(FECHA_ALT,'Month'),LOC, FECHA_ALT     -- TO_DATE 'MM'
             FROM EMPLE E 
                  NATURAL JOIN DEPART D 
                          WHERE EXTRACT(MONTH FROM FECHA_ALT) = (SELECT EXTRACT(MONTH FROM FECHA_ALT) 
                                                                  FROM EMPLE 
                                                                  WHERE SALARIO IN (SELECT MIN(SALARIO)
                                                                                    FROM EMPLE));
     
      --  antesdeayer fue 
      
      SELECT 'Antesdeayer fue ' || (SYSDATE -2) AS ANTEAYER FROM DUAL;
      SELECT CONCAT('ANTEAYER FUE: ', SYSDATE -2) AS ANTEAYER FROM DUAL;
      
      SELECT CONCAT('ANTEAYER FUE : ', TO_CHAR(SYSDATE-2,'Day')) from dual;
                                                                                
      -- Que dia de la semana en el que se dieron de alta los empleados
      
      SELECT APELLIDO, ' Se dio de alta en ' || TO_CHAR(FECHA_ALT,'Day') 
      from emple;
      
      -- Funcion MONTHS BETWEEN
      SELECT APELLIDO,FECHA_ALT, MONTHS_BETWEEN(SYSDATE,FECHA_ALT)/36 FROM EMPLE;
      
      SELECT EXTRACT(YEAR FROM SYSDATE)- 1995 AS EDAD FROM DUAL;
      SELECT 'TENGO: ' || TRUNC(MONTHS_BETWEEN(SYSDATE,'08/09/1995')/12)|| ' años ' from dual;
      
      -- LAST_DAY
      
      SELECT LAST_DAY(SYSDATE)-2 FROM DUAL;
      
      SELECT FECHA_ALT, LAST_DAY(FECHA_ALT) FROM EMPLE;
      SELECT LAST_DAY('01 NOVIEMBRE 95') FROM DUAL;
      
      -- NEXT_DAY
      SELECT NEXT_DAY(SYSDATE,'viernes') as "la fecha del siguiente viernes" FROM DUAL;
      SELECT NEXT_DAY('5/11/2023','MARTES') from dual;
      
      ALTER SESSION SET NLS_DATE_LANGUAGE=ENGLISH; --  con system queda permanente 
            SELECT NEXT_DAY('5/11/2023','thuesday') from dual;

## B.7- GROUP-BY_HAVING


  -- SELECT ---------------
  --   FROM ---------------
  --   [WHERE--------------]
  --   [GROUP BY---}
  --      [HAVING] NECESITA GROUP BY 
  --   [ORDER BY --------]

  -- media salario departamentos
  
  SELECT DEPT_NO,ROUND(AVG(SALARIO)) AS MEDIA
         FROM EMPLE
              GROUP BY DEPT_NO
              ORDER BY MEDIA DESC;
              
  -- MAXIMO SALARIO DE CADA DEPARTAMENTO
  SELECT DEPT_NO, MAX(SALARIO) AS MAXIMO
         FROM EMPLE
          GROUP BY DEPT_NO;
        
  SELECT DEPT_NO,MAX(SALARIO) AS AMAXIMO,MIN(SALARIO) AS MINIMO, ROUND(AVG(SALARIO)) AS MEDIA -- LOS ATRIBUTOS DEL GROUP BY, FUNCIONES DE GRUPO  O CONSTANTES, NADA MAS 
                                                                                              -- EN UN GROUP BY 
         FROM EMPLE
         GROUP BY DEPT_NO;
  -- MEDIAS SALARIALES POR OFICIOS
  
         SELECT ROUND(AVG(SALARIO)),OFICIO
                FROM EMPLE 
                GROUP BY OFICIO;
                
   -- LOCALIDAD DE LOS QUE MENOS GANAN CUYO OFICIO SEA EMPLEADO;
   
   SELECT LOC
          FROM DEPART D
               WHERE DEPT_NO IN (SELECT DEPT_NO
                                 FROM EMPLE
                                 WHERE OFICIO='EMPLEADO'
                                       AND 
                                       SALARIO = (SELECT MIN(SALARIO)
                                                  FROM EMPLE
                                                  WHERE OFICIO='EMPLEADO')) ;
                                                  -- DOS LADOS EMPLEADO PORQUE PUEDE 
                                                  -- GANAR LO MISMO
   -- CUANTOS TRABAJADORES HAY DE CADA OFICIO
   
   SELECT OFICIO,COUNT(*)
          FROM EMPLE
          GROUP BY OFICIO;   
   -- EL DEPARTAMENTO X MEDIA SALARIAL TANTO
   
   SELECT 'El departamento ' ||dept_no ||', media salarial : ' || avg(salario)
          FROM EMPLE 
          GROUP BY DEPT_NO;                                        


-- FALTA LO DEL DOCUMENTO C13

   -- VER SI ALGUN TRABAJADOR GANA LO MISMO QUE LA MEDIA DE SALARIOS DE ALGUN DEPARTAMENTO 
   
   SELECT APELLIDO,SALARIO
          FROM EMPLE
          WHERE SALARIO IN (SELECT round(AVG(SALARIO)) -- O = some (subconsulta)
                           FROM EMPLE
                           GROUP BY DEPT_NO);
                           
   -- apellido y oficio de los que mas ganan de cada oficio
   
   SELECT APELLIDO,OFICIO,SALARIO
          FROM EMPLE
          WHERE (SALARIO,OFICIO) IN (SELECT MAX(SALARIO),OFICIO
                            FROM EMPLE
                            GROUP BY OFICIO);
                            
   -- APELLIDO Y OFICIO DE LOS QUE MAS GANAN DE CADA DEPARTAMENTO
   SELECT APELLIDO,DEPT_NO,SALARIO
          FROM EMPLE
          WHERE (SALARIO,DEPT_NO) IN (SELECT MAX(SALARIO),DEPT_NO
                            FROM EMPLE
                            GROUP BY DEPT_NO);
   -- APELLIDO Y OFICIO DE LOS QUE GANAN MAS O MENOS DE CADA OFICIO
   SELECT APELLIDO,OFICIO,SALARIO
          FROM EMPLE
          WHERE (SALARIO,OFICIO) IN (SELECT MAX(SALARIO),OFICIO 
                            FROM EMPLE
                            GROUP BY OFICIO)
                OR -- NO AND 
                (SALARIO,OFICIO) IN (SELECT MIN(SALARIO),OFICIO
                                 FROM EMPLE
                                 GROUP BY OFICIO)
                           ;
    -- APELLIDO,OFICIO,SALARIO Y LOCALIDAD DE LOS QUE GANAN MAS O MENOS DE CADA OFICIO
      SELECT APELLIDO,OFICIO,SALARIO, LOC
          FROM EMPLE
               NATURAL JOIN DEPART
                            WHERE (SALARIO,OFICIO) IN (SELECT MAX(SALARIO),OFICIO 
                                                       FROM EMPLE
                                                       GROUP BY OFICIO)
                                  OR 
                                  (SALARIO,OFICIO) IN (SELECT MIN(SALARIO),OFICIO
                                                       FROM EMPLE
                                                       GROUP BY OFICIO);
    
       
    -- TOTAL TRABAJADORES Y SUMA SALARIAL DE LOS EMPLEADOS O ANALISTAS ORDENADOS POR SUMA DE MAS A MENOS 
    SELECT DISTINCT OFICIO FROM EMPLE;
    SELECT OFICIO,COUNT(*), ROUND(SUM(SALARIO),2) AS SUMA  -- TENGO QUE REVISAR ESTE , NO LO PILLÉ BIEN
           FROM EMPLE
                WHERE OFICIO IN('EMPLEADO','ANALISTA')
                      GROUP BY OFICIO
                      ORDER BY SUMA DESC;
     -- TOTAL TRABAJADORES Y SUMA SALARIAL DE LOS EMPLEADOS O ANALISTAS SIEMPRE QUE SEAN MAS DE 3
     SELECT OFICIO,COUNT(*),ROUND(SUM(SALARIO),2)
            FROM EMPLE
                 WHERE OFICIO IN ('EMPLEADO','ANALISTA')
                       HAVING COUNT(*) >3              -- COUNT NO PIEDE IR EN UNA WHERE, TIENE QUE IR POR HAVING  .... HAVING PARA FUNCIONES DE GRUPO 
                       GROUP BY OFICIO;
     -- MEDIA SALARIAL Y TOTAL TRABAJADORES DE LOS QUE TRABAJAN EN BARCELONA
     
    SELECT ROUND(AVG(SALARIO),2), COUNT(*)
           FROM EMPLE
                NATURAL JOIN DEPART
                        WHERE LOC='BARCELONA'
                        ;        
     -- TOTAL SALARIO POR OFICIO DE LOS TRABAJADORES DE MADRID
        SELECT SUM(SALARIO),OFICIO
               FROM EMPLE
                    WHERE DEPT_NO = (SELECT DEPT_NO
                                     FROM DEPART 
                                     WHERE LOC='MADRID')
                          GROUP BY OFICIO;
     -- TOTAL SALARIO POR CIUDADES 
     SELECT NVL(TO_CHAR(SUM(SALARIO),'999G999G999G'),0) AS "TOTAL SALARIO POR CIUDADES",LOC
            FROM EMPLE 
                 RIGHT JOIN DEPART  -- 
                 ON DEPART.DEPT_NO=EMPLE.DEPT_NO
                 GROUP BY LOC;
                 
     -- TOTAL EMPLEADOS DE CADA NOMBRE DE DEPARTAMENTO
     
     SELECT DNOMBRE, COUNT(EMP_NO) AS TOTALEMPLEADOS
            FROM EMPLE
                 NATURAL JOIN DEPART
                        GROUP BY DNOMBRE;
     -- TOTAL SALARIO POR CIUDEADES DE LAS CIUDADES CON MAS DE 130000 DE MASA SALARIAL (PONER CERO SI NO HAY SALARIO)
     
     SELECT NVL(SUM(SALARIO),0),LOC
            FROM EMPLE E 
                 RIGHT JOIN DEPART D 
                 ON E.DEPT_NO=D.DEPT_NO
                        GROUP BY LOC
                              HAVING SUM(SALARIO)>1300000;
     -- TOTAL EMPLEADOS DE CADA NOMBRE DE DEPARTAMENTO SIEMPRE QUE LA MEDIA SALARIAL DEL DEPARTAMENTO EXCEDA 250000
     SELECT DNOMBRE, COUNT(EMP_NO), ROUND(AVG(SALARIO),2)
            FROM EMPLE E 
                 RIGHT JOIN DEPART D 
                  ON D.DEPT_NO=E.DEPT_NO
                     GROUP BY DNOMBRE
                           HAVING AVG(SALARIO)>250000;
-- VARIOS ATRIBUTOS DE GROUP BY SEPARADOS POR COMAS 
-- EL PRIMERO ES EL CRITERIO PRIMARIO, LUEGO SE PUEDEN HACER SUBGRUPOS
-- MISMOS REQUISITOS QUE LOS SINGLE GROUP BY , SE PUEDEN USAR FUNCIONES DE GRUPO, CONSTANTES, Y LOS ATRIBUTOS USADOS PARA GROUP BY

   -- MEDIA SALARIAL, NOMBRE DEPARTAMENTO Y LOCALIDAD DE CADA DEPARTAMENTO 
   
   SELECT AVG(SALARIO), DNOMBRE,LOC,DEPT_NO
          FROM EMPLE
               NATURAL JOIN DEPART
                       GROUP BY LOC,DNOMBRE,DEPT_NO;
     
   -- NUM DEPART, NOMBRE LOCALIDAD, TOTAL EMPLEADOS POR DEPARTAMENTO 
   -- ORDENADOS POR NUMERO DE EMPLEADOS
   
   SELECT TRUNC(AVG(SALARIO),2), DNOMBRE,LOC,DEPT_NO , COUNT(EMP_NO) AS TOTALEMPLEADOS
          FROM EMPLE
               NATURAL JOIN DEPART
                       GROUP BY DEPT_NO,LOC,DNOMBRE
                       ORDER BY TOTALEMPLEADOS DESC
                       ;
   -- MISMA CONSULTA PERO CON MAS DE 5 EMPLEADOS
          
     SELECT TRUNC(AVG(SALARIO),2), DNOMBRE,LOC,DEPT_NO , COUNT(EMP_NO) AS TOTALEMPLEADOS
          FROM EMPLE
               NATURAL JOIN DEPART
                       GROUP BY DEPT_NO,LOC,DNOMBRE 
                             HAVING COUNT(EMP_NO)>5 -- NO SE PUEDEN USAR LOS ALIAS AQUI? CASCA 
                       ORDER BY TOTALEMPLEADOS DESC
                       ;       
   -- LOCALIDAD Y SALARIO DE LOS QUE GANAN MAS DE LA MEDIA DE LOS SALARIOS
   SELECT LOC,SALARIO
          FROM EMPLE E
               NATURAL JOIN DEPART D 
                             WHERE SALARIO > (SELECT AVG(SALARIO)
                                              FROM EMPLE);
   -- SALARIO MEDIO Y DEPARTAMENTO DEL DEPARTAMENTO CON MAYOR MEDIA SALARIAL 
   
      SELECT ROUND(AVG(SALARIO)),DEPT_NO
             FROM EMPLE E
                  GROUP BY DEPT_NO
                        HAVING ROUND(AVG(SALARIO)) IN (SELECT  MAX(ROUND(AVG(SALARIO)))  -- TENGO QUE REPASAR ESTE !!!
                                                       FROM EMPLE 
                                                       GROUP BY DEPT_NO);
   
   -- OFICIO CON MENOS TRABAJADORES
   SELECT OFICIO, COUNT(*)
          FROM EMPLE
          GROUP BY OFICIO
                HAVING COUNT(*) = (SELECT MIN(COUNT(*))  -- LAS FUNCIONES DE GRUPO NO SE PUEDEN PONER EN LA WHERE, SOLO HAVING (WHERE COUNT(...)) MAAAL
                                    FROM EMPLE 
                                    GROUP BY OFICIO);
                                    
   -- OFICIO CON MAS IMPORTE TOTLA DE SALARIOS
   
   SELECT SUM(SALARIO), OFICIO
          FROM EMPLE
          GROUP BY OFICIO
                HAVING SUM(SALARIO) = (SELECT MAX(SUM(SALARIO))
                                       FROM EMPLE
                                       GROUP BY OFICIO);
   -- TRABAJADORES QUE TRABAJEN EN UN DEPARTAMENTO CUYA MEDIA SALARIAL SEA A 270.000
   
   SELECT APELLIDO, SALARIO,DEPT_NO
          FROM EMPLE
               GROUP BY DEPT_NO,APELLIDO,SALARIO
                     HAVING DEPT_NO IN (SELECT DEPT_NO
                                        FROM EMPLE
                                         GROUP BY DEPT_NO
                                          HAVING AVG(SALARIO)>270000);
                                       
      -- LOCALIDADES DE LOS DEPARTAMENTOS DONDE TRABAJEN TRABAJADORES EN UN DEPARTAMENTO CUYA MEDIA SALARIAL SEA > 270.000

    SELECT LOC
          FROM EMPLE
               NATURAL JOIN DEPART
               GROUP BY LOC
                     HAVING DEPT_NO IN (SELECT DEPT_NO
                                        FROM EMPLE
                                         GROUP BY DEPT_NO
                                          HAVING AVG(SALARIO)>270000);
   -- SUMA DE SALARIOS, SALARIO MAX Y SALARIO MIN DE CADA DEPARTAMENTO Y POR CADA OFICIO
   
   SELECT SUM(SALARIO),MAX(SALARIO),MIN(SALARIO),DEPT_NO,OFICIO
           FROM EMPLE
                GROUP BY DEPT_NO,OFICIO
                      ORDER BY DEPT_NO DESC;
                      
   -- NOMBRE DE LOS DEPARTAMENTOS CON MAS DE 4 TRABAJADORES
   
      SELECT DNOMBRE, COUNT(*)
             FROM DEPART 
                  NATURAL JOIN EMPLE
                  GROUP BY DNOMBRE
                        HAVING COUNT(*) > 4; -- CON EL JOIN 
      
      SELECT DNOMBRE,DEPT_NO
             FROM DEPART 
              WHERE  DEPT_NO IN (SELECT DEPT_NO
                                      FROM EMPLE
                                      GROUP BY DEPT_NO
                                      HAVING COUNT(*)>4);
                                      
     -- NUMERO MAXIMO DE EMPLEADOS EN ALGUN DEPARTAMENTO
     
     SELECT COUNT(*) ,DEPT_NO
            FROM EMPLE
                 GROUP BY DEPT_NO
                  HAVING COUNT(*) = (SELECT MAX(COUNT(*)) 
                                     FROM EMPLE
                                     GROUP BY DEPT_NO);
     SELECT MAX(COUNT(*))
            FROM EMPLE
            GROUP BY DEPT_NO; -- LO DE ARRIBA ERA INNECESARIO XD
     
     -- CUANTOS TRABAJADORES HAY EN EL OFICIO QUE MAS EMPLEADOS TIENE 
     
     SELECT MAX(COUNT(*))
            FROM EMPLE
            GROUP BY OFICIO;
            
     -- OFICIO CON MAS EMPLEADOS
     
        SELECT OFICIO, COUNT(*)
               FROM EMPLE
                 GROUP BY OFICIO
                       HAVING COUNT(*) IN (SELECT MAX(COUNT(*))
                                           FROM EMPLE
                                           GROUP BY OFICIO); 
     -- n_dept , nombre departamento y numero empleados del que tiene mas empleados 
     
        SELECT DEPT_NO,DNOMBRE, COUNT(EMP_NO)
               FROM DEPART 
                  NATURAL JOIN EMPLE
                       GROUP BY DNOMBRE,DEPT_NO
                             HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                                                FROM EMPLE
                                                GROUP BY DEPT_NO);
                                                
     -- OFICIO CON MAS EMPLEADOS QUE NO SEAN NI EMPLEADO NI VENDEDOR 
     
        SELECT OFICIO, (COUNT(*))
               FROM EMPLE
                    WHERE OFICIO NOT IN ('EMPLEADO','VENDEDOR')
                    GROUP BY OFICIO
                          HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                                             FROM EMPLE
                                             WHERE OFICIO NOT IN('EMPLEADO','VENDEDOR') -- A VOLVER A PONER LA CONDICION , TIENE QUE SER EQUIVALENTE!!! IMPORTANTE IMPORTANTE 
                                             GROUP BY OFICIO);
                                             
     -- DEPARTAMENTO CUYA MEDIA DE SALARIOS ES MAYOR QUE 
     -- LA MEDIA DE TODOS LOS SALARIOS DE TODA LA EMPRESA
     
     SELECT DEPT_NO, AVG(SALARIO)
            FROM EMPLE
                 GROUP BY DEPT_NO 
                       HAVING AVG(SALARIO) > (SELECT AVG(SALARIO)
                                              FROM EMPLE
                                              );
     -- NOMBRE DE LOS DEPARTAMENTOS CON SALARIO MEDIO SUPERIOR A LA MEDIA DE TODA LA EMPRESA 
     
     SELECT DNOMBRE 
            FROM DEPART 
                 WHERE DEPT_NO IN (SELECT DEPT_NO 
                                   FROM EMPLE
                                       GROUP BY DEPT_NO
                                          HAVING SALARIO>AVG(SALARIO)) ; -- NO ME SALE 
     -- MEDIAS SALARIALES POR OFICIO
     
        SELECT ROUND(AVG(SALARIO),2) AS "MEDIAS SALARIALES", OFICIO
               FROM EMPLE
                    GROUP BY OFICIO;
     -- EMPLEADOS QUE GANAN LO MISMO QUE LA MEDIA DE SU OFICIO RESPECTIVO
     
        SELECT APELLIDO
               FROM EMPLE 
                    WHERE (SALARIO,OFICIO) IN (SELECT AVG(SALARIO),OFICIO
                                               FROM EMPLE
                                               GROUP BY OFICIO);
                                        
     -- EMPLEADOS QUE GANAN MENOS QUE LA MEDIA DE SU OFICIO RESPECTIVO
     
        SELECT APELLIDO, SALARIO
               FROM EMPLE E 
                    WHERE SALARIO > (SELECT AVG(SALARIO)    -- CON IGUALDAD NO HACE FALTA, PERO CON MENOR O MAYOR
                                            FROM EMPLE                    -- OFICIO NO PUEDE SER OFICO<OFICIO, POR LO QUE 
                                            WHERE E.OFICIO=OFICIO       -- EN UN WHERE DENTRO 
                                            GROUP BY OFICIO);
                                            
     -- EMPLEADOS QUE GANAN MAS QUE LA MEDIA EN SU DEPARTAMENTO
        
        SELECT APELLIDO, SALARIO
               FROM EMPLE E 
                    WHERE SALARIO > (SELECT AVG(SALARIO)   
                                            FROM EMPLE              
                                            WHERE E.DEPT_NO=DEPT_NO       
                                            GROUP BY DEPT_NO);
                                   
     -- CIUDAD CON MAS EMPLEADOS
     
        SELECT LOC 
              FROM DEPART 
                   WHERE DEPT_NO IN (SELECT DEPT_NO
                                     FROM EMPLE
                                     GROUP BY DEPT_NO
                                           HAVING COUNT(*) = (SELECT MAX(COUNT(*)) 
                                                              FROM EMPLE 
                                                              GROUP BY DEPT_NO)); -- REVISAR 
                                                              
     -- CIUDAD CON MAS TRABAJADORESD DE OFICIO EMPLEADO
     
        SELECT LOC 
               FROM DEPART
               WHERE DEPT_NO IN (SELECT DEPT_NO 
                                 FROM EMPLE
                                      WHERE OFICIO='EMPLEADO'
                                      GROUP BY DEPT_NO
                                       HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                                                          FROM EMPLE
                                                          WHERE OFICIO='EMPLEADO'
                                                          GROUP BY DEPT_NO
                                                          ));
     -- TABLAS PARALEER Y LEIDOS
     SELECT *  FROM PARALEER;
     SELECT * FROM LEIDOS;
     
            SELECT NOMBRE_LIBRO,FECHA,p.COD_LIBRO
                   FROM PARALEER P
                        LEFT JOIN LEIDOS L 
                             ON L.COD_LIBRO=p.cod_libro;
                             
    -- nombre de los empleados con el mismo jefe que TOVAR
    
    SELECT APELLIDO
           FROM EMPLE
                WHERE DIR = (SELECT DIR 
                             FROM EMPLE 
                             WHERE APELLIDO='TOVAR')
                      AND APELLIDO<>'TOVAR';
                      
    -- TOTAL EMPLEADOS A CARGO DE CADA EMPLEADO 
    
    SELECT DIR, COUNT(EMP_NO),APELLIDO
           FROM EMPLE
           GROUP BY DIR,APELLIDO;
                                                              
    -- EMPLEADO QUE SEAN JEFE DE ALGUIEN
    
    SELECT APELLIDO
           FROM EMPLE
                WHERE EMP_NO IN (SELECT DIR 
                                FROM EMPLE);     
    -- EMPLEADOS CON MAS DE DOS A SU CARGO                           
         SELECT APELLIDO
                FROM EMPLE
                WHERE EMP_NO IN (SELECT DIR 
                                 FROM EMPLE
                                 GROUP BY DIR 
                                 HAVING COUNT(*)>2);  
    -- TRABAJADOR QUE TIENE A SU CARGO LA MAYOR SUMA DE SALARIOS DE SUS TRABAJADORES DIRECTOS
    
    SELECT APELLIDO
           FROM EMPLE
           WHERE EMP_NO IN (SELECT DIR
                            FROM EMPLE
                            GROUP BY DIR
                            HAVING SUM(SALARIO) = (SELECT MAX(SUM(SALARIO))
                                                   FROM EMPLE
                                                   GROUP BY DIR));
                                                   
    -- NOMBRE DEL EMPLEADO CON MENOS GENTE A SU CARGO
    
    SELECT APELLIDO
           FROM EMPLE
                WHERE EMP_NO IN (SELECT DIR
                                 FROM EMPLE
                                 GROUP BY DIR 
                                 HAVING COUNT(*) = (SELECT MIN(COUNT(*))
                                                    FROM EMPLE
                                                    GROUP BY DIR));



## B.8- CONNECT-BY

-- PARA RELACIONES REFLEXIVAS    EMP_NO y DIR  de la propia TABLA  MISMA MISMA 
    -- WHERE ....  CONNECT BY.... START WITH
    
    SELECT APELLIDO,EMP_NO,DIR,LEVEL  -- LEVEL SOLO CON CONNECT BY 
           FROM EMPLE
                CONNECT BY PRIOR EMP_NO=DIR -- PRIOR ES RAIZ HACIA FUERA, 
                START WITH APELLIDO='REY'; -- OBLIGATORIO AL PONER CONNECT BY 
                
     SELECT APELLIDO,EMP_NO,DIR,LEVEL
            FROM EMPLE
            WHERE APELLIDO<>'ALONSO'
            CONNECT BY EMP_NO = PRIOR DIR 
                    START WITH APELLIDO='ALONSO'; -- JEFES DE ALONSO 
     -- COLOCACION DE LA JERARQUIA
     SELECT LPAD(APELLIDO,LENGTH(APELLIDO) + 6*LEVEL),OFICIO AS TRABAJADORES, LEVEL
            FROM EMPLE
            CONNECT BY PRIOR EMP_NO=DIR
            START WITH APELLIDO='REY';
      
     -- TODOS LOS QUEDEPENDEN DE JIMENEZ 
      SELECT LPAD(APELLIDO,LENGTH(APELLIDO) + 6*LEVEL),OFICIO AS TRABAJADORES, LEVEL
            FROM EMPLE
            WHERE APELLIDO<>'JIMÉNEZ'
            CONNECT BY PRIOR EMP_NO=DIR
            START WITH APELLIDO='JIMÉNEZ';     
     -- TODOS LOS JEFES DE ALONSO
      SELECT LPAD(APELLIDO,LENGTH(APELLIDO) + 6*LEVEL),OFICIO AS TRABAJADORES, LEVEL
            FROM EMPLE
            WHERE APELLIDO<>'ALONSO'
            CONNECT BY  EMP_NO= PRIOR DIR
            START WITH APELLIDO='ALONSO';  
     -- CUANTOS ESCALAFONES HAY (NIVELES)                                                   
            SELECT COUNT(DISTINCT(LEVEL)) -- O CON MAX LEVEL 
            FROM EMPLE
            WHERE APELLIDO<>'ALONSO'
            CONNECT BY  PRIOR EMP_NO=  DIR
            START WITH APELLIDO='REY';  
            
     -- ESCALAFONES INFERIORES A CEREZO 
      SELECT COUNT(DISTINCT(LEVEL))-1  -- INFERIORES, NO INFERIORES O IGUAL!!!
            FROM EMPLE
            WHERE APELLIDO<>'ALONSO'
            CONNECT BY  PRIOR EMP_NO=  DIR
            START WITH APELLIDO='CEREZO';  
     -- CUANTOS HAY DE CADA ESCALAFON
     
     SELECT COUNT(*), LEVEL
         FROM EMPLE
            CONNECT BY  PRIOR EMP_NO=  DIR
            START WITH APELLIDO='REY'
            GROUP BY LEVEL;  
     -- SUMA DE LOS SALARIOS DE CADA ESCALAFON
     
     SELECT SUM(SALARIO),LEVEL
            FROM EMPLE
            CONNECT BY PRIOR EMP_NO = DIR
            START WITH APELLIDO='REY'
            GROUP BY LEVEL;
      -- CUANTOS HAY CON COMISIONES NULAS EN CADA ESCALAFON 
      
     SELECT * FROM EMPLE;
     
     SELECT COUNT(*), LEVEL
            FROM EMPLE
            WHERE COMISION IS NULL
            CONNECT BY PRIOR EMP_NO = DIR
            START WITH APELLIDO='REY'
            GROUP BY LEVEL;
            
      -- CON NO NULAS SERIA 
      SELECT COUNT(COMISION), LEVEL
             FROM EMPLE
             CONNECT BY PRIOR EMP_NO = DIR
             START WITH APELLIDO='REY'
             GROUP BY LEVEL;
      --SUMA DE SALARIOS DE ANALISTAS DE CADA LEVEL/ESCALAFON
      
      SELECT SUM(SALARIO), LEVEL
             FROM EMPLE
                  WHERE OFICIO='ANALISTA'
                  CONNECT BY PRIOR EMP_NO = DIR
                  START WITH APELLIDO='REY'
                  GROUP BY LEVEL;
      -- SUMA DE SALARIOS Y TOTAL TRABAJADORES QUE SEAN EMPLEADOS DE CADA ESCALAFON
      
      SELECT SUM(SALARIO), COUNT(*), LEVEL
             FROM EMPLE
                  WHERE OFICIO='EMPLEADO'
                       CONNECT BY PRIOR EMP_NO = DIR
                       START WITH APELLIDO='REY'
                       GROUP BY LEVEL; 
      -- TOTAL TRABAJADORES POR ESCALAFON Y DENTRO DE CADA ESCALAFON POR OFICIO 
      
        SELECT LEVEL,COUNT(*), OFICIO
               FROM EMPLE
                       CONNECT BY PRIOR EMP_NO = DIR
                       START WITH APELLIDO='REY'
                       GROUP BY LEVEL,OFICIO
                             ORDER BY LEVEL; 
       -- TOTAL TRABAJADORES POR ESCALAFON Y DENTRO DE CADA ESCALAFON POR DEPARTAMENTO 
       SELECT LEVEL,COUNT(*), DEPT_NO
               FROM EMPLE
                       CONNECT BY PRIOR EMP_NO = DIR
                       START WITH APELLIDO='REY'
                       GROUP BY LEVEL,DEPT_NO
                             ORDER BY LEVEL; 
       -- NUMERO DE TRABAJADORES, ESCALAFON Y OFICIO DEL ESCALAFON CON MAS TRABAJADORES DE OFICIO EMPLEADO
       SELECT LEVEL, OFICIO, COUNT(*)
              FROM EMPLE
                   WHERE OFICIO='EMPLEADO'
                           CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL,OFICIO
                                 HAVING COUNT(*) IN (SELECT MAX(COUNT(*))
                                                    FROM EMPLE
                                                    WHERE OFICIO='EMPLEADO'
                                                    CONNECT BY PRIOR EMP_NO = DIR
                                                    START WITH APELLIDO='REY'
                                                    GROUP BY LEVEL,OFICIO);
       --SACAR LA CODIFICACION Y EL TOTAL DE EMPLEADOS DE CADA CATEGORIA 
       
       SELECT DECODE(LEVEL,1,'EN LA CUSPIDE',  -- CON DECODE, TENGO QUE MIRAR EL CASE 
                           2,'UN PELO Y LLEGAS',
                           3,'PROGRESANDO',
                           4,'CASTA PARIA',
                           'NOSE'
                           ) AS CATEGORIA, LEVEL, COUNT(*) AS TOTAL
                     FROM EMPLE 
                      CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL
                           ORDER BY LEVEL;
              
        SELECT CASE LEVEL
                      WHEN  1 THEN 'OWO'
                      WHEN  2 THEN 'UWU'
                      WHEN  3 THEN 'IWI'
                      WHEN 4 THEN 'EWE'
                        ELSE 'NO COMPUTADO'
                        END AS CATEGORIA, LEVEL, COUNT(*) AS TOTAL
                FROM EMPLE
                CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL
                           ORDER BY LEVEL;
                           
       -- CATALOGA EL NUMERO DE EMPLEADOS MENOS DE 3 POCOS, TRES ACEPTABLE , MENOS DE 7 MUCHOS, OTROS MULTITUD, CONTANDO POR LEVEL
       
        SELECT CASE                           -- NO VA BIEN SI LE PASO PARAMETOR AL CASE, FUNCIONES SIMPLEMENTE A SACO AHÍ 
               WHEN COUNT(*) < 3 THEN 'POCOS'
               WHEN COUNT(*) = 3 THEN 'ACEPTABLE'
               WHEN COUNT(*) < 7 THEN 'MUCHOS' 
               ELSE 'MULTITUD'
                 END AS CATALOGARempleados, LEVEL, COUNT(*) AS TOTAL
                   FROM EMPLE
                CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL
                           ORDER BY LEVEL;
                           
       -- SUMA DE SALARIOS Y TOTAL TRABAJADORES Y CATALOGA COMO: SUMA DE SALARIO <250000 DEMASIADO POCO, <300000 ACEPTABLE Y EL RESTO MUCHO
       -- QUE SEAN OFICIO EMPLEADOS DE CADA ESCALAFON 
       
          SELECT CASE 
                 WHEN SUM(SALARIO)<250000 THEN 'DEMASIADO POCO'
                 WHEN SUM(SALARIO)<300000 THEN 'ACEPTABLE'
                 ELSE 'MUCHO'
                      END AS CATALOGARSALARIOS,LEVEL,COUNT(*),SUM(SALARIO)
                 FROM EMPLE 
                 WHERE OFICIO='EMPLEADO'
                 CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL
                           ORDER BY LEVEL;
                           
       -- ESCALAFON Y TOTAL TRABAJADORES CON MAYOR NUMERO DE TRABAJADORES
       
       SELECT LEVEL AS ESCALAFON, COUNT(*) AS TOTALTRABAJADORES
              FROM EMPLE 
                 CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL
                                 HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                                                    FROM EMPLE 
                                                    CONNECT BY PRIOR EMP_NO = DIR
                                                    START WITH APELLIDO='REY'
                                                    GROUP BY LEVEL);
       -- NOMBRE DE LOS TRABAJADORES DEL ESCALAFON CON MAYOR NUMERO DE TRABAJADORES 
       SELECT APELLIDO,LEVEL
              FROM EMPLE 
                 CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL,APELLIDO -- NO HACE FALTA PONER GROUP BY Y HAVING, CON WHERE LEVEL = (SUBCONSULTAS) VALE 
                                 HAVING LEVEL = (SELECT LEVEL 
                                                 FROM EMPLE
                                                 CONNECT BY PRIOR EMP_NO = DIR
                                                 START WITH APELLIDO='REY'
                                                 GROUP BY LEVEL
                                                 HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                                                                    FROM EMPLE 
                                                                    CONNECT BY PRIOR EMP_NO = DIR
                                                                    START WITH APELLIDO='REY'
                                                                    GROUP BY LEVEL));
                                             
                           
       -- NOMBRE DEPARTAMENTO Y LOCALIDADES DE LOS TRABAJADORES DEL ESCALAFON CON MAYOR NUMERO DE TRABAJADORES
       
       SELECT APELLIDO,LEVEL, DNOMBRE,LOC -- SIN APELLIDO SE HACE SIN JOIN Y CON DISTINCT 
              FROM EMPLE 
                   NATURAL JOIN DEPART                            -- TENGO QUE MIRAR LA DEL PROFE C 17
                                 WHERE LEVEL = (SELECT LEVEL 
                                                 FROM EMPLE
                                                 CONNECT BY PRIOR EMP_NO = DIR
                                                 START WITH APELLIDO='REY'
                                                 GROUP BY LEVEL
                                                 HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                                                                    FROM EMPLE 
                                                                    CONNECT BY PRIOR EMP_NO = DIR
                                                                    START WITH APELLIDO='REY'
                                                                    GROUP BY LEVEL))
                             CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY';
                           
         -- NOMBRE DEPARTMAENTO Y APELLIDOS DE LOS TRABAJADORES DEL ESCALAFON CON MENOR NUMERO DE TRABAJADORES
           SELECT APELLIDO, DNOMBRE -- SIN APELLIDO SE HACE SIN JOIN Y CON DISTINCT 
              FROM EMPLE 
                   NATURAL JOIN DEPART                              -- enunciado raro raro , supuestamente tienen que salir 3 no 1 
                                 WHERE LEVEL = (SELECT LEVEL 
                                                 FROM EMPLE
                                                 CONNECT BY PRIOR EMP_NO = DIR
                                                 START WITH APELLIDO='REY'
                                                 GROUP BY LEVEL
                                                 HAVING COUNT(*) = (SELECT MIN(COUNT(*))
                                                                    FROM EMPLE 
                                                                    CONNECT BY PRIOR EMP_NO = DIR
                                                                    START WITH APELLIDO='REY'
                                                                    GROUP BY LEVEL))
                             CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY';
         -- ESCALAFON CON MAYOR NUMERO DE EMPLEADOS DE COFICIDADO 
         SELECT CASE LEVEL
                      WHEN  1 THEN '4444'
                      WHEN  2 THEN '22'
                      WHEN  3 THEN 'I99898WI'
                      WHEN 4 THEN '1111'
                        ELSE 'NO COMPUTADO'
                        END AS CATEGORIA, LEVEL, COUNT(*) AS TOTAL
                FROM EMPLE
                CONNECT BY PRIOR EMP_NO = DIR
                           START WITH APELLIDO='REY'
                           GROUP BY LEVEL
                                 HAVING COUNT(*) =   (SELECT MAX(COUNT(*)) 
                                                                                     FROM EMPLE
                                                                                     CONNECT BY PRIOR EMP_NO = DIR
                                                                                     START WITH APELLIDO='REY'
                                                                                     GROUP BY LEVEL);
     -- APELLIDO DE LOS TRABAJADORES DEL ESCALAFON CON MAYOR NUMERO DE EMPLEADOS 
     
      SELECT APELLIDO,OFICIO, LEVEL 
                FROM EMPLE                                   -- ENTENDER LA CONSULTA, PARA NO HACER MAS COMPLEJO EL PRIMER PASO 
                     WHERE  LEVEL = (SELECT LEVEL 
                                                 FROM EMPLE 
                                                 CONNECT BY PRIOR EMP_NO = DIR
                                                 START WITH APELLIDO='REY'
                                                 GROUP BY LEVEL
                                                       HAVING COUNT(*) =  (SELECT MAX(COUNT(*)) 
                                                                                     FROM EMPLE
                                                                                     CONNECT BY PRIOR EMP_NO = DIR
                                                                                     START WITH APELLIDO='REY'
                                                                                            GROUP BY LEVEL))
                      CONNECT BY PRIOR EMP_NO = DIR
                      START WITH APELLIDO='REY';
     
     -- OFICIOS DONDE TRABAJAN LOS TRABAJADORES CON EL NIVEL CON MENOS TRABAJADORES
     
        SELECT OFICIO, APELLIDO,LEVEL
               FROM EMPLE
               WHERE (LEVEL,OFICIO) = (SELECT LEVEL,OFICIO
                              FROM EMPLE 
                              CONNECT BY PRIOR EMP_NO=DIR
                              START WITH APELLIDO='REY'
                              GROUP BY LEVEL,OFICIO
                                    HAVING COUNT(*) = (SELECT MIN(COUNT(*))
                                                                FROM EMPLE
                                                                CONNECT BY PRIOR EMP_NO=DIR
                                                                START WITH APELLIDO='REY'
                                                                GROUP BY LEVEL,OFICIO))
                     CONNECT BY PRIOR EMP_NO=DIR
                              START WITH APELLIDO='REY';
                 
       -- ESCALAFON DE SALA 
       SELECT * FROM EMPLE;
       
       SELECT LEVEL 
              FROM EMPLE
              WHERE APELLIDO='SALA'
                   CONNECT BY PRIOR EMP_NO=DIR
                   START WITH APELLIDO='REY';
       -- CUANTOS TRABAJADORES TIENEN EL MISMO NIVEL QUE SANCHEZ 
       
       SELECT COUNT(*)
              FROM EMPLE
              WHERE LEVEL = (SELECT LEVEL 
                                    FROM EMPLE
                                    WHERE APELLIDO='SÁNCHEZ'
                                    CONNECT BY PRIOR EMP_NO=DIR
                                    START WITH APELLIDO='REY')
                    AND APELLIDO<>'SÁNCHEZ'
                               CONNECT BY PRIOR EMP_NO=DIR
                               START WITH APELLIDO='REY';

## B.9- SELECT-EXPRESSION

 -- ************************* SELECT EXPRESSION  ********************************
   -- ***************************************************************************
   -- SINTAXIS
   -- SELECT (SELECT  {REQUISITO DE QUE 1 SOLO ATRIBUTO Y DEVUELVA UNA SOLA TUPLA} ), (SELECT.....)
   -- FROM....
   
      SELECT (SELECT DEPT_NO   -- SOLO 1 ATRIBUTO!!
              FROM EMPLE
              WHERE APELLIDO='SÁNCHEZ')
                , 
              (SELECT DNOMBRE 
              FROM DEPART
              WHERE LOC='MADRID'
              ) 
              FROM DUAL;
              
            -- MIRAR APUNTES 17-11 PARA LO QUE MUESTRA 
    -- USER Y UID
      SELECT 'EL USUARIO ES ' || 
             (SELECT USER 
              FROM DUAL) ||
              '  Y MI UID ES ' || 
              (SELECT UID 
              FROM DUAL)
             FROM DUAL;
    -- SALARIO MAXIMO Y MINIMO
    
    SELECT 'SALARIO MAXIMO: ' || 
           (SELECT MAX(SALARIO)
            FROM EMPLE)  ||
            '    SALARIO MINIMO: ' ||
            (SELECT MIN(SALARIO) 
            FROM EMPLE)AS "MÁXIMO Y MINIMO"
            FROM DUAL;
            
    -- DEPARTAMENTO CON MAS EMPLEADOS Y DEPARTAMENTO CON MENOS EMPLEADOS
    
       SELECT 'DEPARTAMENTO CON MAS EMPLEADOS: ' ||
              (SELECT DEPT_NO                                      -- MAS INTERESANTE CON DOS ATRIBUTOS EN VEZ DE CONFCAT
               FROM EMPLE
               GROUP BY DEPT_NO
               HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                                  FROM EMPLE 
                                  GROUP BY DEPT_NO)) ||
               '----------------DEPARTAMENTO CON MENOS EMPLEADOS: ' ||
               (SELECT DEPT_NO 
               FROM EMPLE
               GROUP BY DEPT_NO
               HAVING COUNT(*) = (SELECT MIN(COUNT(*))
                                  FROM EMPLE 
                                  GROUP BY DEPT_NO)) AS "MAXIMO Y MINIMO"
              FROM DUAL;
                  
     -- CUANTOS EMPLEADOS HAY DE BARCLEONA Y DE MADRID
     
     SELECT (SELECT COUNT(EMP_NO)
             FROM EMPLE
                  NATURAL JOIN DEPART                           --MIRAR SIN JOIN UNIENDO POR DEPT_NO
                  WHERE LOC='BARCELONA') AS BARCELONA,
             (SELECT COUNT(EMP_NO)
              FROM EMPLE
                  NATURAL JOIN DEPART
                  WHERE LOC='MADRID') AS MADRID
           FROM DUAL;

      -- OFICIO CON MAS SUMA SALARIAL Y DEPARTAMENTO CON MAS SUMA DE SALARIOS
      
      SELECT (SELECT OFICIO 
              FROM EMPLE
              GROUP BY OFICIO
              HAVING SUM(SALARIO) = (SELECT MAX(SUM(SALARIO))
                                     FROM EMPLE 
                                     GROUP BY OFICIO)) AS "OFICIO CON MAS SUMA SALARIAL"
             ,
             (SELECT DEPT_NO
              FROM EMPLE
              GROUP BY DEPT_NO
              HAVING SUM(SALARIO) = (SELECT MAX(SUM(SALARIO))
                                     FROM EMPLE 
                                     GROUP BY DEPT_NO))
             FROM DUAL;
             
       -- OFICIO DE REY Y SALARIO DE GIL 
       
       SELECT ( SELECT OFICIO
                FROM EMPLE
                WHERE APELLIDO='REY') AS "OFICIO DE REY"
                ,
                (SELECT SALARIO
                FROM EMPLE
                WHERE APELLIDO='GIL') AS  "SALARIO DE GIL"
       FROM DUAL;
       
       -- SALARIO DE GIL, OFICIO DE TOVAR Y LOCALIDAD DEL DEPARTAMENTO 20 
       
       SELECT (SELECT SALARIO FROM EMPLE WHERE APELLIDO='GIL') AS "SALARIO DE GIL",
              (SELECT OFICIO FROM EMPLE WHERE APELLIDO='TOVAR') AS "OFICIO DE TOVAR",
              (SELECT LOC FROM DEPART WHERE DEPT_NO=20) AS "LOCALIDAD DEL DEPARTAMENTO 2O"
              FROM DUAL;
        
       -- JEFE DE GIL Y FECHA DE CALCULO ACTUAL 
       SELECT * FROM EMPLE;
       SELECT (SELECT APELLIDO FROM EMPLE WHERE EMP_NO = (SELECT DIR FROM EMPLE WHERE APELLIDO='GIL')) AS "JEFE DE GIL",
              (SELECT SYSDATE FROM DUAL) AS "FECHA ACTUAL"
              FROM DUAL;
                              
       -- MEDIA SALARIAL Y TOTAL DE TRABAJADORES DE LA EMPRESA 
       
       SELECT (SELECT ROUND(AVG(SALARIO),2) FROM EMPLE) AS "MEDIA SALARIAL" ,
              (SELECT COUNT(EMP_NO) FROM EMPLE) AS "TOTAL"
              FROM DUAL;
              
       -- OFICIO CON MAYOR MASA SALARIAL Y DEPARTAMENTO CON MAYOR MASA SALARIAL. y FECHA DE CALCULO
       
       SELECT (SELECT OFICIO FROM EMPLE GROUP BY OFICIO HAVING SUM(SALARIO) = (SELECT MAX(SUM(SALARIO)) FROM EMPLE GROUP BY OFICIO)) AS "OWO",
              (SELECT DEPT_NO FROM EMPLE GROUP BY DEPT_NO HAVING SUM(SALARIO) = (SELECT MAX(SUM(SALARIO)) FROM EMPLE GROUP BY DEPT_NO))AS UWU,
              (SELECT SYSDATE FROM DUAL) -- MIRAD LA SOLUCION DE SANTI PARA LA FECHA 
              FROM DUAL;
              
       -- ¿TIENEN SALA Y TOVAR EL MISMO OFICIO? 
       SELECT 'SI' 
              FROM DUAL
              WHERE (SELECT OFICIO FROM EMPLE WHERE APELLIDO='SALA') = (SELECT OFICIO FROM EMPLE WHERE APELLIDO='TOVAR');
              
       -- "SI Y SON : OFICIO" 
       
       SELECT 'SI Y SON : ' || (SELECT DISTINCT(OFICIO) FROM EMPLE WHERE APELLIDO IN('SALA','TOVAR')) AS RESPUESTA 
                FROM DUAL
              WHERE (SELECT OFICIO FROM EMPLE WHERE APELLIDO='SALA') = (SELECT OFICIO FROM EMPLE WHERE APELLIDO='TOVAR');
       -- TRABAJAN SANCHEZ Y GIL EN EL MISMO DEPARTAMENTO?
       
       SELECT 'SI EN EL DEPARTAMENTO ' || (SELECT DISTINCT(DEPT_NO) FROM EMPLE WHERE APELLIDO IN('SÁNCHEZ','GIL')) AS RESPUESTA
              FROM DUAL 
                   WHERE (SELECT DEPT_NO FROM EMPLE WHERE APELLIDO='GIL') = (SELECT DEPT_NO FROM EMPLE WHERE APELLIDO='SÁNCHEZ');
## B.10- CONJUNTOS


SELECT * FROM ALUM;
SELECT * FROM NUEVOS;
SELECT * FROM ANTIGUOS;

SELECT NOMBRE FROM PROFE.ALUM
    UNION
    SELECT NOMBRE FROM PROFE.NUEVOS;
SELECT NOMBRE FROM ALUM
    UNION ALL
    SELECT NOMBRE FROM NUEVOS;

SELECT NOMBRE FROM ALUM
    INTERSECT                        -- MEJOR INTERSECT QUE "IN" ES MUCHO MAS RAPIDO , SIEMPRE QUE SE PUEDA 'INTERSECT'
    SELECT NOMBRE FROM NUEVOS; 
--TAMBIEN
SELECT NOMBRE FROM ALUM
    WHERE NOMBRE IN (SELECT NOMBRE FROM NUEVOS);

SELECT NOMBRE,LOCALIDAD FROM ALUM
    MINUS                            -- SE LE QUITAN LAS COMUNES , ES COMO UN NOT IN DEL SELECT , MAS RAPIDO QUE NOT IN 
                                     -- LA SEGUNDA NO PUEDE LLEVAR ALIAS 
     SELECT NOMBRE,LOCALIDAD FROM ANTIGUOS
           ORDER BY NOMBRE;
--TAMBIEN
 SELECT NOMBRE,LOCALIDAD FROM ALUM
      WHERE NOMBRE NOT IN (SELECT NOMBRE FROM ANTIGUOS)
    ORDER BY NOMBRE;
 
 

--1 ALUMNOS EN ALUMNOS, QUE ESTEN EN NUEVOS Y NO EN ANTIGUOS
 SELECT NOMBRE FROM ALUM
        INTERSECT  
        (SELECT NOMBRE FROM NUEVOS
        MINUS
        SELECT NOMBRE FROM ANTIGUOS);
     
 -- 2. NOMBRE DE ALUMNOS QUE ESTEN EN NUEVOS O ALUMNOS QUE ESTEN EN ANTIGUOS
 
   SELECT NOMBRE FROM ALUM            -- CON IN PERO NO ES OPTIMO
     WHERE NOMBRE IN (SELECT NOMBRE FROM ANTIGUOS)
           OR NOMBRE IN (SELECT NOMBRE FROM NUEVOS);
           
   SELECT NOMBRE FROM ALUM                           -- LOS PARENTESIS INFLUYEN!!! SIN PARENTESIS HACE INTERSECT CON ANTIGUOS Y LUEGO A ESO LE HACE UNION CON TODO NUEVOS
          INTERSECT (SELECT NOMBRE FROM ANTIGUOS
                          UNION
                     SELECT NOMBRE FROM NUEVOS);
  --   ALUMNOS EN ANTIGUOS O EN ALUMNOS PERO NO EN NUEVOS 
  
   SELECT NOMBRE FROM ALUM
          UNION 
                 SELECT NOMBRE FROM ANTIGUOS 
                   MINUS 
                   SELECT NOMBRE FROM NUEVOS;
   
  -- ALUMNOS QUE ESTEN ALUMNOS Y EN NUEVOS O ESTEN SOLO EN ANTIGUOS
  
  SELECT NOMBRE FROM ALUM 
         INTERSECT 
           SELECT NOMBRE FROM NUEVOS
               UNION 
            SELECT NOMBRE FROM ANTIGUOS;
            
  -- NOMBRES DE ALUMNOS QUE NO ESTEN EN EMPLE 
   
  SELECT NOMBRE FROM ALUM
         MINUS SELECT APELLIDO FROM EMPLE; // HA METIDO ANA EN SU CODIGO 
         


    -- APELLIDOS Y LOCALIDADES DE LOS TRABAJADORES DE LAS DOS EMPRESAS 
    
    SELECT APELLIDO,LOC FROM EMPLE
        NATURAL JOIN DEPART 
            UNION 
                SELECT NOMBRE,LOCALIDAD FROM OTROEMPLE;
            
  -- 27/11/2023
        
         -- TRABAJADORES QUE TRABAJAN EN MADRID EN AMBAS EMPRESAS
   -- por hacer             
   
  
   -- todos los que no trabajen en madrid en las dos empresas 
      SELECT APELLIDO,EMP_NO
             FROM EMPLE
             WHERE DEPT_NO NOT IN (SELECT DEPT_NO
                                   FROM DEPART
                                   WHERE LOC<>'MADRID')
                   UNION
                   
                   SELECT NOMBRE,DNI
                          FROM OTROEMPLE
                          WHERE LOCALIDAD<>'MADRID';
                          
    -- TRABAJADORES QUE TRABAJEN EN LAS DOS EMPRESAS
    
       SELECT APELLIDO FROM EMPLE
       INTERSECT 
              SELECT NOMBRE FROM OTROEMPLE;
       
    -- TRABAJADORES QUE SOLO TIENEN UN TRABAJO
       SELECT APELLIDO FROM EMPLE
              UNION 
              SELECT NOMBRE FROM OTROEMPLE
                     MINUS
                     (SELECT APELLIDO FROM EMPLE
                             INTERSECT 
## B.11- INSERT/DELETE

   
      -- DECODE (EXPRESION,
      --          VALOR1 , SALIDA1
      --          VALOR2SALIDA2,....,LOS QUE HAGAN FALTA 
      --          RESTO)
      
      SELECT OFICIO, DECODE(OFICIO,
                            'PRESIDENTE','President',
                            'VENDEDOR','Salesman',
                            'EMPLEADO','Employee',
                            'No sé') as "DECODIFICACION DEL OFICIO" -- PODEMOS PONER OFICIO o concatenacion para que saque el oficio original o con mensaje
                            FROM EMPLE;
                            
      select * from libreria;
         -- codificar biologia por bicho y dibujo por diseño
         
         SELECT TEMA, DECODE(TEMA,'Biología       ','Bicho','Biología       ','Diseño', TEMA) -- SE PUEDE HACER RTRIM DEL PRIMER PARAMETRO TEMA
          AS "NUEVO NOMBRE" from LIBRERIA;       
          
         -- CASE EXPRESSION
          --       WHEN VALOR THEN SALIDA
          --       WHEN VALOR2 THEN SALIDA2
          --       ELSE 
                 -- RESTO
          -- END;
          
          -- EN SQL MEJOR DECODE, EN PLSQL CASE
          
          -- CON CASE
           SELECT OFICIO, (CASE OFICIO
                            WHEN 'PRESIDENTE' THEN 'President'
                            WHEN 'VENDEDOR' THEN 'Salesman'
                            WHEN 'EMPLEADO' THEN 'Employee'
                            ELSE 'No sé' 
                             END) as "DECODIFICACION DEL OFICIO"  
                            FROM EMPLE;
             SELECT TEMA, (CASE RTRIM(TEMA)
                           WHEN 'Biología' THEN 'Bicho'
                           WHEN 'Dibujo' THEN 'Diseño'
                           ELSE TEMA ||' xd'
                           END) AS "Nuevo Tema"
                           from libreria;
          -- que sesion esta conectada? funcion USER
          
          SELECT USER FROM DUAL;
          --PK DONDE SE GUARDA ID DE USUARIO
          SELECT UID FROM DUAL;
          
          -- CUANTOS EMPLES SE DIERON DE ALTA EN EL DIA MEDIO DE ALTA DE LA EMPRESA (4 DIAS DESPUES PARA QUE SALA ALGUIEN
          
          SELECT COUNT(*) 
                 FROM EMPLE
                  WHERE EXTRACT(DAY FROM FECHA_ALT) = (SELECT ROUND(AVG(EXTRACT (DAY FROM FECHA_ALT)))+4
          -- PARA FORMATEAR . EN CANTIDADES
          -- TO_CHAR(SALARIO*100000,'999G999G999G999G')                                               FROM EMPLE);
                  


        -- EJERCICIOS
    --1 
    
    SELECT DEPT_NO
        FROM EMPLE
        GROUP BY DEPT_NO 
            HAVING AVG(SALARIO) >= (SELECT AVG(SALARIO)
                                    FROM EMPLE);
    --2
    SELECT COUNT(*)
        FROM EMPLE 
            WHERE DEPT_NO = (SELECT DEPT_NO
                             FROM DEPART
                             WHERE DNOMBRE='VENTAS')
                  AND OFICIO='VENDEDOR';
    --3 
    
    SELECT SUM(SALARIO),OFICIO
        FROM EMPLE
            WHERE DEPT_NO = (SELECT DEPT_NO
                            FROM DEPART
                            WHERE DNOMBRE='VENTAS')
            GROUP BY OFICIO; -- POR QUE NO VALE GROUP BY Y HAVING? REPASO
            
    --4
    
    SELECT APELLIDO 
        FROM EMPLE E
            WHERE (SALARIO) = (SELECT AVG(SALARIO)
                                        FROM EMPLE
                                        WHERE DEPT_NO=E.DEPT_NO);
    --5
    
    SELECT COUNT(*)AS "NUMERO EMPLEADOS",DEPT_NO
        FROM EMPLE
            WHERE OFICIO='EMPLEADO'
            GROUP BY DEPT_NO
            ORDER BY DEPT_NO;
    
    --6 
    
    SELECT DEPT_NO 
        FROM EMPLE
            WHERE OFICIO='EMPLEADO'
        GROUP BY DEPT_NO
            HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                               FROM EMPLE 
                               WHERE OFICIO='EMPLEADO'
                               GROUP BY DEPT_NO);
                
    --7
    
      SELECT E.DEPT_NO,DNOMBRE
        FROM EMPLE E
            INNER JOIN DEPART D 
            ON D.DEPT_NO=E.DEPT_NO
            WHERE OFICIO='EMPLEADO'
        GROUP BY E.DEPT_NO,DNOMBRE
            HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                               FROM EMPLE 
                               WHERE OFICIO='EMPLEADO'
                               GROUP BY DEPT_NO);
    --8 
    

## B.12- 2EVTIPOS DE DATOS
	14-12-2023 
	Toda sentencia DDL tiene implicita un commit work.
	El nombre de un objeto tiene una serie de requisitos:
		-max 30 caracteres
		-empezar por una letra 
		-puede haber numeros
		-los unicos caracteres "raros" : _ $   nada más
		-No coincidir con un palabras reservadas
		-Hay excepciones, pero no puede haber dos objetos del mismo nombre
		-USUARIO.NOMBRE_OBJETO (dos usuarios si pueden tener una tabla llamada emple)

	CREATE TABLE NOMBRE
		(NOMBRE CAMPO1 TIPO_DATO,
		C2 TIPO_DATO,
		C3 TIPO_DATO,
		-------
		) [TABLESPACE NOMBRE]; SI NO SE ESPECIFICA, SE CREA EN EL TABLESPACE POR DEFECTO DEL USUARIO
															SE MIRA CON SELECT *  FROM USER_USERS;

	DROP TABLE NOMBRETABLA;

	CREATE TABLE NOSE(NUM NUMBER(8,2), NOM VARCHAR2(10)); 2 DECIMALES
	--DATOS GRANDES
		CREATE TABLE NOSE 
		(NUM NUMBER,
		FOTO1 LONG RAW,
		FOTO2 LONG RAW
		); -- NO DEJA, SOLO PUEDE HACER UNA COLUMNA DE TIPO LONG (DA IGUAL SI ES LONG RAW+LONG, SON TIPO LONG Y MAS DE 1 CASCAN)
		-- CON BLOB NO
		CREATE TABLE NOSE
		(NUM NUMBER
		FOTO1 BLOB,
		FOTO2 BLOB
		); -- 2 BLOB DEJA DEJA.  Y 1 BLOB Y UN CLOB DEJA TAMBIÉN 

	SELECT * FROM USER_TABLES
		WHERE TABLE_NAME LIKE 'NOSE%';
	SELECT * FROM USER_CATALOG
		WHERE TABLE_NAME LIKE 'NOSE%';
	SELECT * FROM dba_CATALOG;
	SELECT * FROM USER_OBJECTS
		WHERE OBJECT_NAME LIKE 'EMPLE';

	CREATE TABLE NOSE3
	(DATO CHAR(2000));
	INSERT INTO NOSE3 VALUES ('W');
	DELETE FROM NOSE3; -- REQUIERE COMMIT WORK
	TRUNCATE TABLE NOMBRE_TABLA [REUSE STORAGE / DROP STORAGE (POR DEFECTO)]; BORRA TODAS LAS TUPLAS MAS RAPIDO QUE DELETE SIN WHERE
	-- TRUNCATE TIENE IMPLICITA EL COMMIT WORK Y ES MUCHO MAS RAPIDA PARA MUCHAS FILAS
	-- ADEMAS DE BORRAR, LAS OPCIONES SON: APARTE DE LAS TUPLAS, ESE ESPACIO LO BORRAS (DROP) O LO DEJAS RESERVADO (REUSE) 

	SELECT * FROM USER_SEGMENTS 
		WHERE SEGMENT_NAME = 'NOSE3';
	SELECT * FROM USER_EXTENDS
		WHERE SEGMENT_NAME ='NOSE3';
-- RESTRICCIONES
	PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, DEFAULT 
	restricciones = CONSTRAINT 
	CON O SIN NOMBRE  (si no se le pone, lo pone ORACLE)
	A NIVEL DE ATRIBUTO (PARA CADA ATRIBUTO AL DECLARARLO) O 
	A NIVEL DE TABLA (AL FINAL). 
	Momento de validación: al concluir cada una de las operaciones individuales 
	o cuando acabe la transacción.

	Sintaxis a nivel de atributo 
		CREATE TABLE NOMBRE
		(C1 TIPO1 [CONSTRAINT NOMBRE1] TIPO RESTRICCION [INITIALLY DEFERRED/INMEDIATE] 
		[CONSTRAINT NOMBRE2] .............................
		,
		C2 TIPO2 
		) [TABLESPACE AKWOQKEOQWE];

	CREATE TABLE SUJETOS
	(NOM VARCHAR2(20),
	NIF VARCHAR2(10) PRIMARY KEY,
	EDAD NUMBER
	) TABLESPACE USUDAW;
	
	SELECT INTO SUJETOS VALUES('nose','11-a',22);
	SELECT INTO SUJETOS VALUES('owo','11-a',49); casca, PK funciona

*SELECT * FROM USER_CONSTRAINTS
	WHERE TABLE_NAME='SUJETOS';
SELECT * FORM USER_CONS_COLUMNS
	WHERE TABLE_NAME='SUJETOS';
SELECT * FROM USER_TAB_COLUMNS
	WHERE TABLE_NAME='SUJETOS';*

CREATE TABLE DETALLE 
	(COD_FACTURA NUMBER PRIMARY KEY , - CASCA, SOLO 1 PK
	COD_PRODUCTO NUMBER PRIMARY KEY,
	UNIDADES NUMBER
	); 
*CON RESTRICCIONES A NIVEL DE TABLA*
CREATE TABLE DETALLE 
	(COD_FACTURA NUMBER ,
	COD_PRODUCTO NUMBER  , 
	UNIDADES NUMBER,  -- COMA AUNQUE ULTIMO ATRIB
	CONSTRAINT PK_DETALLE   -- PK CONJUNTA 
		PRIMARY KEY(COD_FACTURA,COD_PRODUCTO) INITIALLY INMEDIATE
	) TABLESPACE USUDAW; 
*DEFERRED* hasta el commit no da error 
CREATE TABLE DETALLE 
	(COD_FACTURA NUMBER ,
	COD_PRODUCTO NUMBER  , 
	UNIDADES NUMBER,  -- COMA AUNQUE ULTIMO ATRIB
	CONSTRAINT PK_DETALLE   -- PK CONJUNTA 
		PRIMARY KEY(COD_FACTURA,COD_PRODUCTO) INITIALLY DEFERRED
	) TABLESPACE USUDAW; 

*SELECT * FROM USER_INDEXES
	WHERE TABLE_NAME='DETALLE';*


	
	
		
# C- EJERCICIOS
## C.1- EJ1_CONSULTAS

- *1* A partir de la tabla EMPLE, visualiza cuántos apellidos de los empleados empiezan por
	la letra 'A':
	
				SELECT COUNT( * )
					FROM EMPLE
						WHERE APELLIDO LIKE 'A%' 
							AND OFICIO LIKE '%E%';



    
- *2* Selecciona el apellido, el oficio y la localidad de los departamentos donde trabajan
	los analistas
	
				SELECT APELLIDO,OFICIO,LOC
					FROM EMPLE
						NATURAL JOIN DEPART
							WHERE OFICIO='ANALISTA';
   
 
- *3* Mostrar apellido,oficio,salario y fecha de alta de los empleados que desempeñan el mismo oficio que JIMÉNEZ o que tengan un salario mayor o igual que FERNÁNDEZ

				  SELECT APELLIDO,OFICIO,SALARIO,FECHA_ALT
					  FROM EMPLE
						  WHERE (OFICIO = (SELECT OFICIO
											  FROM EMPLE
												  WHERE APELLIDO='JIMÉNEZ')
								OR 
								SALARIO >= (SELECT SALARIO
												FROM EMPLE
													WHERE APELLIDO='FERNÁNDEZ'))
								AND APELLIDO NOT IN('JIMÉNEZ','FERNÁNDEZ');
- *4* Mostrar nombre,oficio y salario de los empleados del dept_no de fernandez que tengan su mismo salario

				   SELECT APELLIDO,OFICIO,SALARIO
				          FROM EMPLE  
				            WHERE (DEPT_NO,SALARIO) = (SELECT DEPT_NO,SALARIO
				                             FROM EMPLE 
				                             WHERE APELLIDO='FERNÁNDEZ')
				                  AND APELLIDO<>'FERNÁNDEZ';
-  *5* Nombres y oficios de los empleados que tienen el mismo trabajo que JIMÉNEZ 

				   SELECT APELLIDO,OFICIO
					   FROM EMPLE
						   WHERE OFICIO = (SELECT OFICIO
											   FROM EMPLE 
												   WHERE APELLIDO='JIMÉNEZ')
							AND 
								APELLIDO<>'JIMÉNEZ';
          
-  *6* Tema, estante y ejemplares de las filas de libreria con ejemplares entre 8 y 15

				   SELECT TEMA, ESTANTE, EJEMPLARES
				          FROM LIBRERIA
					          WHERE EJEMPLARES BETWEEN 8 AND 15;
          
- *7*  Tema,estante y ejemplares de las filas con estante no  comprendido enter la B y la D

				SELECT TEMA,ESTANTE,EJEMPLARES
					FROM LIBRERIA
						WHERE ESTANTE NOT BETWEEN 'B' AND 'D';
    
- *8* Temas de Libreria cuyo numero de ejemplares sea inferior a los que hay en Medicina

				SELECT TEMA
					FROM LIBRERIA
						WHERE EJEMPLARES < (SELECT EJEMPLARES
												FROM LIBRERIA
													WHERE TEMA='Medicina');
    
- *9* Visualizar TEMAS de libreria cuyo numero de ejemplares no esté enter 15 y 20, ambos incluidos

				SELECT TEMA,EJEMPLARES
					FROM LIBRERIA 
						WHERE EJEMPLARES NOT BETWEEN 15 AND 20;
				   
- *10* Asignaturas que contengan tres letras o en su interior y tengan alumnos matriculados en Madrid

			SELECT NOMBRE
				FROM ASIGNATURAS
					WHERE NOMBRE LIKE '%o%o%o%'
						AND COD IN (SELECT COD
										FROM NOTAS
											WHERE DNI IN (SELECT DNI 
														 FROM ALUMNOS
														 WHERE POBLA='Madrid'));				   
- *11*  Nombres de los alumnos que tengan una nota entre 7 y 8  en FOL

			SELECT APENOM 
				FROM ALUMNOS
					WHERE DNI IN (SELECT DNI
									FROM NOTAS
										WHERE NOTA BETWEEN 7 AND 8
											AND 
											COD = (SELECT COD 
													FROM ASIGNATURAS
													WHERE NOMBRE='FOL'));

- *12* Nombres de asignaturas que no tengan suspensos  

			SELECT NOMBRE 
				FROM ASIGNATURAS 
					WHERE COD NOT IN (SELECT COD
									FROM NOTAS
										WHERE NOTA<5);

- *13* Nombres de alumnos de 'Madrid' que tengan alguna asignatura suspendida

			SELECT APENOM 
				FROM ALUMNOS
					WHERE DNI IN (SELECT DNI 
									FROM NOTAS
										WHERE NOTA<5);

- *14* Nombres de los alumnos que tengan al misma nota que tiene 'Díaz Fernández, María' en FOL en alguna asignatura 

		SELECT APENOM
			FROM ALUMNOS
			WHERE DNI IN (SELECT DNI
						FROM NOTAS 
						WHERE NOTA = (SELECT NOTA
										FROM NOTAS
								 		WHERE DNI = (SELECT DNI
													FROM ALUMNOS
										WHERE APENOM='Díaz Fernández, María')
											AND
											COD = (SELECT COD 
													FROM ASIGNATURAS
													WHERE NOMBRE='FOL')))
				AND APENOM<>'Díaz Fernández, María';


## C.2- EJ2_FUNCIONES

- *2* Emple, cuantos apellidos de empleados empiezan por 'A'
			SELECT COUNT( * ) 
				FROM EMPLE
					WHERE UPPER(APELLIDO) LIKE 'A%';
- *3* EMPLE, sueldo medio, numero de comisiones no nulas, maximo sueldo y el minimo, de los empleados del departamento 30.
	SELECT AVG(SALARIO),COUNT(COMISION),MAX(SUELDO),MIN(SUELDO)
		FROM EMPLE
			WHERE DEPT_NO = 30;

   
--4 S
    SELECT COUNT(*) 
           FROM LIBRERIA 
           WHERE TEMA LIKE '%a%';
--5 

    SELECT TEMA
           FROM LIBRERIA 
           WHERE EJEMPLARES = (SELECT MAX(EJEMPLARES)
                               FROM LIBRERIA
                               )
                 AND
                     TEMA LIKE '%e%';
--6 
    SELECT COUNT(DISTINCT ESTANTE)
           FROM LIBRERIA;
--7
   SELECT COUNT(DISTINCT ESTANTE) 
          FROM LIBRERIA 
          WHERE TEMA LIKE '%e%';

--8
   SELECT * FROM MISTEXTOS;
   SELECT RPAD(TITULO,32,'-^')
          FROM MISTEXTOS;
--9 

          
--10
          -- INSTR Y SUBSTRING  EL SEGUNDO ESPACIO
--11 
   SELECT * FROM LIBROS;
   
--13
   SELECT TITULO
          FROM LIBROS
          ORDER BY LENGTH(TITULO);
-- 14
   SELECT * FROM NACIMIENTOS;
   
   SELECT (RTRIM(NOMBRE)|| ' nació el ' || EXTRACT(DAY FROM FECHANAC) ||' de ' || TO_CHAR(FECHANAC,'Month')||' de '|| EXTRACT(YEAR FROM FECHANAC))
      FROM NACIMIENTOS;
      
--15 
   SELECT TEMA,LENGTH(RTRIM(TEMA)),SUBSTR(TEMA,LENGTH(RTRIM(TEMA)))
          FROM LIBRERIA;
          
--16 
   SELECT * FROM NACIMIENTOS; 
   
   SELECT (RTRIM(NOMBRE)||'       ' || TO_CHAR(FECHANAC,'DD ') || TO_CHAR(FECHANAC,'Month')
   || TO_CHAR(FECHANAC,'Year')) AS "FECHA FORMATEADA"
          FROM NACIMIENTOS;  
   
   
 -- el enunciado de dos maneras es con translate y con LOWER(LTRIM(RTRIM)
 
 
 -- 17 
    SELECT TO_CHAR(TO_DATE('01051998','DDMMYYYY'),'DD MONTH YYYY') FROM DUAL;

-- 18

SELECT * FROM LIBRERIA;

       SELECT (CASE EJEMPLARES
              WHEN 7 THEN 'SEVEN'
  
              END)
              FROM LIBRERIA;
- *19* 
	SELECT *
		FROM empleados
			WHERE MONTHS_BETWEEN(SYSDATE, fecha_alt) / 12 >= 19;

-- 21 
	SELECT APELLIDO,SALARIO,DEPT_NO
        FROM EMPLE E
            WHERE SALARIO IN (SELECT MAX(SALARIO)
                                FROM EMPLE 
                                    WHERE DEPT_NO=E.DEPT_NO
                                    );
--22

     SELECT APELLIDO,SALARIO, DEPT_NO
            FROM EMPLE E 
            WHERE 
                  SALARIO > (SELECT AVG(SALARIO) 
                             FROM EMPLE
                             WHERE E.DEPT_NO=DEPT_NO);

## C.3- EJ3_?

- *1* Los departamentos en los que el salario medio es >= a la media de todos
	SELECT DEPT_NO
    FROM EMPLE
        GROUP BY DEPT_NO
            HAVING AVG(SALARIO) > (SELECT AVG(SALARIO)
                                    FROM EMPLE);
- *8* 
	    SELECT DEPT_NO
        FROM EMPLE
            GROUP BY DEPT_NO
                HAVING COUNT(OFICIO) > (SELECT COUNT(DISTINCT(OFICIO))
                                        FROM EMPLE);
## C.4- EJ4_



# D- 2_APUNTES_SUCIO
### *08/01/24*


             CREATE TABLE NOSE11
             (DNI VARCHAR2(10) PRIMARY KEY,
             NOM VARCHAR(20)
             ) TABLESPACE USUDAW;
             -- COSAS DE DICCIONARIOS (apuntes, ya lo vimos)  user_cons_columns,  user_ttab_cols
             DROP TABLE NOSE11 PURGE; 
             CREATE TABLE NOSE11
             (DNI VARCHAR2(10) CONSTRAINT PK_NOSE11 PRIMARY KEY,
             NOM VARCHAR(20)
             ) TABLESPACE USUDAW;             

             SELECT * FROM USER_CONSTRAINTS
                    WHERE TABLE_NAME='NOSE11';  
                                
             CREATE TABLE NOSE11  -- casca, no es la manera de hacer pk conjunta
             (
             NUMF NUMBER PRIMARY KEY, 
             CODP NUMBER PRIMARY KEY,
             UNIDADES NUMBER
             ) TABLESPACE USUDAW;
             
           CREATE TABLE NOSE11  -- 
             (
             NUMF NUMBER, 
             CODP NUMBER,
             UNIDADES NUMBER,
             
             CONSTRAINT PK_NOSE11 
                        PRIMARY KEY(NUMF,CODP) INITIALLY IMMEDIATE -- POR DEFECTO, VALIDA ASI, INMEDIATAMENTE
             ) TABLESPACE USUDAW;
             
                CREATE TABLE NOSE11  -- A NIVEL DE TABLA, NO DE ATRIBUTO PARA HACER PK CONJUNTA 
             (
             NUMF NUMBER, 
             CODP NUMBER,
             UNIDADES NUMBER,
             
             CONSTRAINT PK_NOSE11 
                        PRIMARY KEY(NUMF,CODP) INITIALLY DEFERRED -- CUANDO CONCLUYAMOS LA TRANSACCION, ERROR. AL COMMIT -> CASCA
             ) TABLESPACE USUDAW;
        
             -- UNIQUE 
             -- NO PERMITE DUPLICADOS, ES OPCIONAL, PUEDE HABER VARIOS UNIQUE EN UNA TABLA
             DROP TABLE NOSE11 PURGE;
             CREATE TABLE NOSE11
             (
             NUMF NUMBER,
             CODP NUMBER,
             UNIDADES NUMBER,
             DATO NUMBER CONSTRAINT U_DATO_NOSE11 UNIQUE NOT NULL, --PARA NO OPCIONAL, UNIQUE NOT NULL
             DATO2 NUMBER CONSTRAINT U_DATO2_NOSE11 UNIQUE,
             DATO3 NUMBER NOT NULL,
             CONSTRAINT PK_NOSE11
                        PRIMARY KEY (NUMF,CODP) INITIALLY IMMEDIATE
              ) TABLESPACE USUDAW;
              
             INSERT INTO NOSE11 VALUES (1,10,2000,22,111);
             INSERT INTO NOSE11 VALUES (1,11,2000,23,333);
             INSERT INTO NOSE11 VALUES (2,10,2000,24,111); -- CASCA
             INSERT INTO NOSE11 (NUMF,CODP,DATO,DATO3) VALUES (1,11,2000,25,666);-- OPCIONALES, HAY QUE ESPECIFICAR SI NO SON TODOS, 
             
             -- RESTRICCION "CHECK" , es una pequenia validacion
             DROP TABLE NOSE11 PURGE;
             CREATE TABLE NOSE11
             (NOM VARCHAR2(20) CONSTRAINT C_NOM_NOSE11 CHECK(LENGTH(NOM)=5),
             EDAD NUMBER CONSTRAINT C_EDAD_NOSE11 CHECK (EDAD<=100 AND EDAD>=18)
             ) TABLESPACE USUDAW;
             
             INSERT INTO NOSE11 VALUES ('DIEGO',15);-- CASCA, RESTRICCION VIOLADA, NO EN EL RANGO DE EDAD
             INSERT INTO NOSE11 VALUES ('PEPE',19);-- CASCA, RESTRICCION VIOLADA, NO 5 LETRAS
             
             CREATE TABLE NOSE11
             (NOM VARCHAR2(20),
             SALARIO1 NUMBER NOT NULL,
             SALARIO2 NUMBER NOT NULL,
             
             CONSTRAINT C_SALARIOS_NOSE11
                        CHECK((SALARIO1 + SALARIO2)<100)
                        INITIALLY DEFERRED
                        )
             TABLESPACE USUDAW;
             INSERT INTO NOSE11 VALUES('PEPE',10,20);
             INSERT INTO NOSE11 VALUES('OLGA',90,20);-- CASCAM 110 ES MAYOR QUE LA RESTRICCION
             
             
             
             -- EJERCICIO , TABLA PARA GUARDAR NOMBRE, DNI Y CORREO ELECTRONICO, Y LA CIUDAD, QUE SEA ALCALA, MADRID O TORREJON
             DROP TABLE NOSE11 PURGE;
             CREATE TABLE NOSE11 
             (NOMBRE VARCHAR2(20) CONSTRAINT C_NOMBRE_NOSE11 CHECK(LENGTH(NOMBRE)!=5),
             DNI NUMBER(8) ,
             CORREO VARCHAR2(20) CONSTRAINT C_CORREO_NOSE11 CHECK(CORREO LIKE '%@%.%') CHECK ( CORREO LIKE '%_@_%._%' AND SUBSTR (CORREO, INSTR(CORREO,'.'+1) IN ('ES','COM','ORG'))),
             CIUDAD VARCHAR2(20) CONSTRAINT C_CUDAD_NOSE11 CHECK (CIUDAD IN('TORREJON','MADRID','ALCALA','GUADALAJARA')),
             CONSTRAINT PK_NOSE11
                        PRIMARY KEY (DNI) INITIALLY IMMEDIATE)
                    TABLESPACE USUDAW;
                    
             INSERT INTO NOSE11 VALUES('SARA',09089908,'owo@gmail.com','TORREJON');

###  *11/01/24*

DEFAULT, si no damos un valor a un campo, se queda a null. 

	CREATE TABLE NOSE12
		(NUM NUMBER,
		FECHA DATE DEFAULT SYSDATE
		USUARIO VARCHAR2(30) DEFAULT USER
		);
	INSERT INTO NOSE12 VALUES (1,'12/12/2023');
	INSERT INTO NOSE12(NUM) VALUES(2); - HAY QUE DECIR EL QUE INSERTAMOS

DEFAULT *NO* ADMITE FUNCIONES DE SQL NI ATRIBUTOS DE TABLA, NI CONSTRAINT
SI ADMITE SYSDATE, USER,UID    ->  CHECK NO LAS PUEDE UTILIZAR 
NO A NIVEL DE TABLA
SI UN ATRIBUTO TIENE VARIAS RESTRICCIONES
DEFAULT NO PUEDE SER LA ÚLTIMA RESTRICCION (SALVO QUE SEA LA ÚNICA)
NOT NULL TAMPOCO PUEDE ESTAR A NIVEL DE TABLA 

	CREATE TABLE NOSE12
	(NUM NUMBER,
	EDAD NUMBER CONSTRAINT C_EDAD_NOSE12
			CHECK (EDAD BETWEEN 20 AND 80)
			DEFAULT 40    -- CASCA PORQUE NO PUEDE SER LA ÚLTIMA
	);

	CREATE TABLE NOSE12
		(NUM NUMBER,
		EDAD NUMBER DEFAULT 40   -- BIEN
				CONSTRAINT C_EDAD_NOSE12
				CHECK (EDAD BETWEEN 20 AND 80)
		);

	CREATE TABLE NOSE12
	(NUM NUMBER,
	EDAD NUMBER,
	
	CONSTRAINT C_EDAD_NOSE12
		CHECK (EDAD BETWEEN 20 AND 80) -- CHECK A NIVEL DE TABLA BIEN 
	);
	
	CREATE TABLE NOSE12
		(NUM NUMBER,
		EDAD NUMBER,   -> SIN COMA ESTARIA A NIVEL DE ATRIBUTO Y FUNCIONA, CON COMA ES A NIVEL DE TABLA, CASCA 
		DEFAULT 40     -- NO SE LE PUEDE PASAR NI SU PROPIO ATRIBUTO
		CONSTRAINT C_EDAD_NOSE12
			CHECK (EDAD BETWEEN 20 AND 80) -- CHECK A NIVEL DE TABLA BIEN 
		);
**Foreign Key** 
COCHES y PERSONAS , SI CADA COCHE TUVIERA UN PROPIETARIO

	CREATE TABLE PERSONAS
	(DNI VARCHAR2(20) CONSTRAINT PK_PERSONAS 
					PRIMARY KEY,
	 NOM VARCHAR2(30) CONSTRAINT C_NOM 
					CHECK (NOM LIKE '%E%')
	);
	
	CREATE TABLE COCHES
	(MATRICULA VARCHAR2(7) CONSTRAINT PK_COCHES
					PRIMARY KEY
					 CONSTRAINT C_COCHES
					 CHECK (LENGTH(MATRICULA)=7),
	FECHA_COMPRA DATE,
	DNII VARCHAR2(20) CONSTRAINT FK_COCHES_PERSONAS
					REFERENCES PERSONAS(DNI)     -- NO TIENEN POR QUE LLAMARSE IGUAL
	);

	PARA VER LAS RESTRICCIONES DE LAS TABLAS
	SELECT * FROM USER_CONSTRAINTS 
		WHERE TABLE_NAME IN('PERSONAS','COCHES'); -- MARCA CON UNA R LA FK , DE REFERENCES 
		
	DROP TABLE PERSONAS CASCADE CONSTRAINT PURGE;

	ON DELETE CASCADE EN LA CONSTRAINT

	UPDATE PERSONAS 
	SET DNI='22'
		WHERE DNI='1'; -- NO DEJA PORQUE TIENE COCHES, Y NO PUEDE CAMBIAR EN COCHES A HACER UN UPDATE CASCADE A 22 EN MYSQL SI

A NIVEL DE TABLA
	(PONER COCHES Y PERSONAS CONSTRAINTS A NIVEL DE TABLA) A MIRAR EN APUNTES FK ESPECIALMENTE !!

si es de n a n la relacion coches-pesonas , se crea personas, coches, y una nueva tabla con las pk's de las dos tablas

	CREATE TABLE PERSONAS
	(DNI VARCHAR2(20) CONSTRAINT PK_PERSONAS 
					PRIMARY KEY,
	 NOM VARCHAR2(30) CONSTRAINT C_NOM 
					CHECK (NOM LIKE '%E%')
	);
	
	CREATE TABLE COCHES
	(MATRICULA VARCHAR2(7) CONSTRAINT PK_COCHES
					PRIMARY KEY
					 CONSTRAINT C_COCHES
					 CHECK (LENGTH(MATRICULA)=7),
	FECHA_COMPRA DATE,
	);

	CREATE TABLE TITULARIDADES
	(DNI VARCHAR2(20),
	FECHA DEFAULT SYSDATE,
	MATRICULA VARCHAR2(7) CONSTRAINT C_COCHES
					 CHECK (LENGTH(MATRICULA)=7),  -- REDUNDANTE, MATRICULA EN COCHES YA ESTA
	
	CONSTRAINT PK_TITULARIDADES PRIMARY KEY(DNI,MATRICULA),
	CONSTRAINT FK_TITU_PERSONAS FOREIGN KEY(DNI) REFERENCES PERSONAS(DNI),
	CONSTRAINT FK_TITU_COCHES FOREIGN KEY(MATRICULA) REFERENCES COCHES(MATRICULA)
	);
	CONSTRAINT NOMBRES UNICOS AUNQUE PERTENEZCAN A DISTINTAS TABLAS



### *12/01/24*
     CLIENTES       PRODUCTOS     
         |             |
        1:N            |
         |            1:N
      FACTURAS          |   
         |1:N          |
         |             |
      DETALLE___________| PK CONJUNTA DE LAS DOS REFERENCIAS A FACTURAS Y PRODUCTOS (FK) (NO TIENE POR QUE EN 1:N POR QUE SER CONJUNTA)
      
      DROP TABLE CLIENTES CASCADE CONSTRAINT PURGE;
      DROP TABLE FACTURAS CASCADE CONSTRAINT PURGE; 
      DROP TABLE PRODUCTOS CASCADE CONSTRAINT PURGE;
      DROP TABLE DETALLES PURGE;
      
      CREATE TABLE CLIENTES 
       (DNI VARCHAR2(10) CONSTRAINT PK_CLIENTES  PRIMARY KEY,
       NOM VARCHAR2(30) 
       );
       
       CREATE TABLE FACTURAS
       (NUMF NUMBER CONSTRAINT PK_FACTURAS PRIMARY KEY,
       FECHA DATE,
       DNI VARCHAR2(10) CONSTRAINT FK_FACTURAS_CLIENTES 
                                   REFERENCES CLIENTES(DNI)
                                   ON DELETE CASCADE
       );
       
       CREATE TABLE PRODUCTOS 
       (COD NUMBER CONSTRAINT PK_PRODUCTOS PRIMARY KEY,
       PVP NUMBER(8,2),
       EXISTENCIAS NUMBER
       );
       
       CREATE TABLE DETALLES 
       (UNIDADES NUMBER,
       NUMF NUMBER CONSTRAINT FK_DETALLES_FACTURAS 
                             REFERENCES FACTURAS(NUMF)
                             ON DELETE CASCADE,  -- se pone en la Foreign Key el ON DELETE CASCADE!!
       COD NUMBER CONSTRAINT FK_DETALLES_PRODUCTOS 
                             REFERENCES PRODUCTOS(COD)
                             ON DELETE CASCADE
       );
  INSERT INTO CLIENTES VALUES('1','UNO');
  INSERT INTO CLIENTES VALUES('2','DOS');
  
  INSERT INTO FACTURAS VALUES(10,SYSDATE,1);
  INSERT INTO FACTURAS VALUES(11,'12/10/2020',2);
  INSERT INTO FACTURAS VALUES(12,SYSDATE,1);
  
  INSERT INTO PRODUCTOS VALUES (100,2,200);
  INSERT INTO PRODUCTOS VALUES (101,22,200);
  INSERT INTO PRODUCTOS VALUES (102,24,500);
  INSERT INTO PRODUCTOS VALUES (103,72,400);
  
  INSERT INTO DETALLES VALUES (2,10,100);
  INSERT INTO DETALLES VALUES (1,10,101);
  INSERT INTO DETALLES VALUES (1,10,102);
  INSERT INTO DETALLES VALUES (2,11,100);
  
  SELECT * FROM FACTURAS;
  SELECT * FROM CLIENTES;
  SELECT * FROM PRODUCTOS;
  SELECT * FROM DETALLES;
  
  DELETE FROM CLIENTES WHERE DNI='1';  -- DEMOSTRACION DEL ON DELETE CASCADE

### *15/01/24*
*----------------------------------------  ATRIBUTOS VIRTUALES --------------------------------*
Los atributos virtuales no gastan espacio en disco. Desventaja : hay que procesar la expresión para calcularlos. Estan disponibles 
Tantos atributos virtuales como queramos, en cualquier orden. Lo normal es ponderlos al final 
	CREATE TABLE AOSKDOQWE
	(C TIPO.........,
	C2 TIPO..........,
	ATRIBVIRTUAL [TIPO DE DATO] GENERATED ALWAYS AS (EXPRESION SQL) VIRTUAL
	) [ TABLESPACE ASDQWE ] ; 


  CREATE TABLE VIRTUAL1
  (NOM VARCHAR2(20) CONSTRAINT PK_VIRTUAL1 
                    PRIMARY KEY,
   SALARIO NUMBER CONSTRAINT C_SALARIO_VIRTUAL1
                             CHECK (SALARIO<=100)
                             NOT NULL,
   COMISION NUMBER,
   TOTAL NUMBER GENERATED ALWAYS AS (SALARIO+ NVL(COMISION,0)) VIRTUAL
   ) TABLESPACE USUDAW;
   
   INSERT INTO VIRTUAL1 VALUES('DIEGO',20,45,65); -- CASCA, NO SE PUEDEN DAR VALORES A LOS ATRIBUTOS VIRTUALES 
   INSERT INTO VIRTUAL1 VALUES('DIEGO',20,45); -- CASCA, NO HAY VALORES SUFICIENTES
   INSERT INTO VIRTUAL1(NOM,SALARIO,COMISION) VALUES('DIEGO',20,45); -- HAY QUE ESPECIFICAR CUALES
   
   SELECT * FROM VIRTUAL1;
   
   SELECT * FROM USER_TAB_COLS 
          WHERE TABLE_NAME='VIRTUAL1';
          
   --UNA TABLA CON CAMPOS: DESCRIPCION (SI ES UNA MESA  -> CODIGO1, SILLA-> CODIGO2, CUADRO -> CODIGO3 ) + 1 GUION + LONGITUD DE LA DESCRIPCION(MESA,SILLA...)
   
        CREATE TABLE DESCRIPCION1 (
           DNI NUMBER PRIMARY KEY,
           DESCRIPCION VARCHAR2(20) CONSTRAINT C_DESC_DESCRIPCION1 CHECK(DESCRIPCION IN('MESA','SILLA','CUADRO')),
           CODIGO VARCHAR2(50) GENERATED ALWAYS AS (
              DECODE (DESCRIPCION,
                 'MESA', 'CODIGO1-',
                 'SILLA', 'CODIGO2-' ,
                 'CUADRO', 'CODIGO3-',
                 DESCRIPCION
              ) || '-' ||  LENGTH(DESCRIPCION)
           ) VIRTUAL
        );

      DROP TABLE DESCRIPCION1 PURGE;
   
      INSERT INTO DESCRIPCION1(DNI,DESCRIPCION) VALUES(1,'MESA');
            INSERT INTO DESCRIPCION1(DNI,DESCRIPCION) VALUES(2,'SILLA');
                  INSERT INTO DESCRIPCION1(DNI,DESCRIPCION) VALUES(3,'CUADRO');


      
      SELECT * FROM DESCRIPCION1;

CCC : 
EEEE - ENTIDAD
SSSS - Nº SUCURSAL
CC
NNNNNNNNNN NUMERO CUENTA

IBAN: ES34CCC
;

      CREATE TABLE VIRTUAL3 
      (DNI VARCHAR2(9) PRIMARY KEY,
      NOM VARCHAR2(20),
      IMPORTE NUMBER,
      CCC VARCHAR2(20),
      ENTIDAD GENERATED ALWAYS AS (SUBSTR(CCC,1,4)) VIRTUAL,
      SUCURSAL GENERATED ALWAYS AS (SUBSTR(CCC,5,4)) VIRTUAL,
      CODIGO_CONTROL GENERATED ALWAYS AS (SUBSTR(CCC,9,2)) VIRTUAL,
      NUM_CUENTA GENERATED ALWAYS AS (SUBSTR(CCC,11,10)) VIRTUAL
      ) TABLESPACE USUDAW2;
     
      INSERT INTO CCC(DNI,NOM,IMPORTE,CCC) VALUES(09,'DIEGO',5000,'10203040506070809010');
      INSERT INTO CCC(DNI,NOM,IMPORTE,CCC) VALUES(08,'DIEGO',5000,'95090901221312382585');
      
      DROP TABLE VIRTUAL3 PURGE;
      
      SELECT * FROM VIRTUAL3;

      

### *18/01/24*

Copiando tablas con 

	CREATE TABLE NOSE223344 AS SELECT * FROM NOSE2233;    *no se copian las constraint que tengan nombre personalizado* 
	
La copia no tiene por qué ser igual 

	CREATE TABLE EMPLEDOS
	AS SELECT EMP_NO,APELLIDO,SALARIO
	FROM EMPLE
	WHERE OFICIO='ANALISTA';

	CREATE TABLE EMPLEDOS
	AS SELECT EMP_NO, APELLIDO,NVL(SALARIO,0) + NVL(COMISION,0)        *CASCA*
	FROM EMPLE
	
	CREATE TABLE EMPLEDOS
	AS SELECT EMP_NO, APELLIDO,NVL(SALARIO,0) + NVL(COMISION,0)  AS TOTAL - 1. CON ALIAS VA BIEN
	FROM EMPLE
	
	CREATE TABLE EMPLEDOS(NUMERO,NOMBRE,TOTAL)  - 2. ESPECIFICANDO LOS CAMPOS
	AS SELECT EMP_NO, APELLIDO,NVL(SALARIO,0) + NVL(COMISION,0)       
	FROM EMPLE

	
	  CREATE TABLE DEPARTAMENTO1(DEPARTAMENTO,MEDIA)
	         AS SELECT DEPT_NO, TRUNC(AVG(SALARIO),2)
	            FROM EMPLE
	                 GROUP BY DEPT_NO;
	        DROP TABLE DEPARTAMENTO1 PURGE;
	                 SELECT * FROM DEPARTAMENTO1;
	                 
	  CREATE TABLE DEPARTAMENTO2(APELLIDO,OFICIO,LOCALIDAD)
	         AS SELECT APELLIDO,OFICIO,LOC
	            FROM EMPLE 
	                 NATURAL JOIN DEPART;
	                 
	            SELECT * FROM DEPARTAMENTO2;
        
	 *ALTER*  - 
	ALTER TABLE NOMBRE       ->  ADD (COLUMNA TIPO [ RESTRICCIONES ], COL2 TIPO2 [ RESTRICCIONES ] ........ );
	                        DROP  (COLUMNA, COLUMNA2  ........ );
	                        MODIFY (COL TIPO [ RESTRICCIONES ], COL2 TIPO [ RESTRICCIONES ] ); -- MODIFICAR TIPO O RESTRICCIONES
	                        ADD CONSTRAINT NOMBRE_RESTRICCION   RESTRICCION  (A COLUMNAS YA EXISTENTES) DE UNA EN UNA
	                        DROP CONSTRAINT NOMBRE ; DE UNA EN UNA 
	                        ENABLE CONSTRAINT NOMBRE ; 
	                        DISABLE CONSTRAINT NOMBRE ;
	CREATE TABLE PRUEBA
	(X NUMBER(5),
	XX VARCHAR2(10)
	);
	
	ALTER TABLE PRUEBA 
	ADD XXX NUMBER, XXXX NUMBER);

	ALTER TABLE PRUEBA DROP(XXX,XXXX);

	ALTER TABLE PRUEBA MODIFY (X NUMBER(10)) ;   SIN DATOS TODO VA BIEN PERO CUANDO HAY DATOS...

	ALTER TABLE PRUEBA MODIFY (XX VARCHAR2(6)) ; SI EL DATO QUE HAY ESTA DENTRO DE LA DIMENSION, NP, SI NO, NO DEJA EN VARCHAR2
										     CON NUMBER DEJA AUMENTARLO, PERO NUNCA REDUCIRLO, AUNQUE EL NUMBER ENTRE

	SE PUEDEN AÑADIR RESTRICCIONES SI NO HAY DATOS, SIN PROBLEMA  , SI TIENE DATOS? DEPENDE DE SI LA VIOLAN O NO

	ALTER TABLE PPRUEBA MODIFY (X NUMBER (5) ) CONSTRAINT PK_PRUEBA
										PRIMARY KEY
										);

	ALTER TABLE PRUEBA ADD CONSTRAINT PK_PRUEBA 
						PRIMARY KEY( X ) ;
						
	SELECT * FORM USER_CONSTRAINTS 
		WHERE TABLE_NAME = PRUEBA ;
		
	ALTER TABLE PRUEBA DROP CONSTRAINT PK_PRUEBA;

	CREATE TABLE PRUEBA
	(X NUMBER(5),
	XX VARCHAR2(10),
	CORREO VARCHAR2(20)
	);
	NOT NULL CON UN MODIFY *
	
	ALTER TABLE PRUEBA MODIFY(CORREO VARCHAR2(20) NOT NULL); FUNCIONA AL ESTAR VACIA 
	ALTER TABLE PRUEBA ADD ( CORREO VARCHAR2(30) NOT NULL) ; LE AÑADES LA COLUMNA NOT NULL, Y SI YA TIENE OTROS DATOS CASCA
	
	CON UN CHECK SE PUEDE NO DEJAR VACIO ESE ATRIBUTO :
	ALTER TABLE PRUEBA ADD (CORREO VARCHAR2(30) 
						CONSTRAINT C_CORREO_PREBA 
						CHECK CORREO IS NOT NULL)
						);
						
	CREATE TABLE FALLOS
	(FILA ROWID,
	USU VARCHAR2(30),
	TAB VARCHAR2(30),
	CON VARCHAR2(30)
	);

	ALTER TABLE PRUEBA ENABLE CONSTRAINT PK_PRUEBA
		EXCEPTIONS INTO FALLOS;

DEJA AÑADIR UN ATRIBUTO VIRTUAL ¿? EN C5 ,   SI 
		
*PARTICIONAMIENTO*
	CREATE TABLE TABLAV
	(DNI VARCHAR2(30),
	NOM VARCHAR2(20),
	APE1 VARCHAR2(30),
	APE2 VARCHAR2(30)
	) TABLESPACE USUDAW2;

### *19/01/24*
	PARTICIONAMIENTO 
	PRIMARY KEY, FOREIGN KEY, Y LUEGO OTRA CLAVE ES LA CLAVE DE PARTICIONAMIENTO. CON ELLA INDICAMOS (PUEDE SER COMPUESTA)
	QUE LOS DATOS QUE TENGAN UNA RELACIÓN, SE JUNTEN FÍSICAMENTE. HAY QUE PONER UN BUEN CRITERIO DE BUSQUEDA PARA LA CLAVE DE PARTICIONAMIENTO

- PARTICIONAMIENTOS POR *RANGO*     -   BY RANGE
	CLAVE PARTICIONAMIENTO: DATE , NUMBER, VARCHAR2
	PUEDE SER COMPUESTA

	CREATE TABLE ..........
	........................................
	.......................................
	[TABLESPACE] ...........
	[PARTITION.................] ;  EN CUALQUIER ORDEN PARTITION Y TABLESPACE
	
	CREATE TABLE ALUMNOS
	(DNI VARCHAR2(10) 
		CONSTRAINT PK_ALUMNOS
		PRIMARY KEY,
	NOMBRE VARCHAR2(20),
	EDAD NUMBER,
	CODIGO NUMBER
	)
	PARTITION BY RANGE(CODIGO) 
		(PARTITION UNO VALUES LESS THAN (2),
		PARTITION DOS VALUES LESS THAN (3),
		PARTITION TRES LESS THAN (4)
		)
	TABLESPACE USUDAW;

	SELECT * FROM USER_TABLES 
		WHERE TABLE_NAME= 'ALUMNOS' ;
	SELECT * FROM USER_TAB_PARTITIONS;

	SELECT * FROM ALUMNOS PARTITION (TRES) ; (DESPUES DE HABER INSERTADO DATOS) CONSULTAR SIN WHERE, MAS RAPIDO CUANDO HAY PARTICION
		se puede poner una where luego
	SELECT * FORM ALUMNOS PARTITION (TRES) 
		WHERE NOMBRE = 'UNO33';
 *si metes un codigo 7, fuera del rango de la partition, hay que hacer ajustes o no te deja*
 
CREATE TABLE ALUMNOS
	(DNI VARCHAR2(10) 
		CONSTRAINT PK_ALUMNOS
		PRIMARY KEY,
	NOMBRE VARCHAR2(20),
	EDAD NUMBER,
	CODIGO NUMBER
	)
	PARTITION BY RANGE(CODIGO) 
		(PARTITION UNO VALUES LESS THAN (2),
		PARTITION DOS VALUES LESS THAN (3),
		PARTITION TRES LESS THAN (4),
		PARTITION OTROS VALUESS LESS THAN(MAXVALUE)
		)
	TABLESPACE USUDAW;

		// empleados, nombre salario  menos de 100 en una, menos de 1000 en otra y menos de 10.000 en otra
		
		CREATE TABLE EMPLEADOS
		(NOMBRE VARCHAR2(20)
		SALARIO NUMBER
		)
		TABLESPACE  USUDAW  -- SI INDICO TABLESPACE, TODAS A ESE, SI NO, TODAS AL DEFAULT PERO!! PUEDE IR CADA UNA A UN TABLESPACE
		PARTITION BY RANGE(SALARIO)
		(PARTITION POBRES VALUES LESS THAN (100),
		PARTITION MEDIO VALUES LESS THAN (1000),
		PARTITION RICOS VALUES LESS THAN(10000)
		PARTITION OTROS VALUES LESS THAN(MAXVALUE)
		;
	// ventas, nif, nombre, importe venta y fecha. vamos a particionar por trimestre
	CREATE TABLE FACTURAS
	(NIF(VARCHAR2)
	NOMBRE VARCHAR2
	IMPORTE NUMBER,
	FECHA DATE)
	PARTITION BY RANGE(FECHA)
	(PARTITION PRIMER_TRIMESTRE VALUES LESS THAN (TO_DATE('1/ABRIL/2023','dd/MON/yyyy'))
												TABLESPACE USUDAW,
	PARTITION SEGUNDO_TRIMESTRE VALUES LESS THAN (TO_DATE('1/JUL/2023','dd/MON/yyyy'))
												TABLESPACE USUDAW2,
	PARTITION TERCER_TRIMESTRE VALUES LESS THAN (TO_DATE('1/OCT/2023','dd/MON/yyyy')),
												TABLESPACE USUDAW,
	PARTITION CUARTO_TRIMESTRE VALUES LESS THAN (TO_DATE('1/ENE/2024','dd/MON/yyyy''))
												TABLESPACE USUDAW2
	) ; 

	SELECT * FROM FACTURAS PARTITION (CUARTO_TRIMESTRE) ; // despues de insertar 
	


	las papeleras : SELECT * FROM USER_RECYCLEBIN;      purge una tabla no va a la papelera, no se puede recuperar
	PURGE USER_RECYCLEBIN; //para limpiar   si esta particionada se borran la especificacion global de  tabla, el indice, y cada una de las particiones
	como se recupera de la papelera: 
	FLASHBACK TABLE FACTURAS TO BEFORE DROP;
	FLASHBACK TABLE FACTURAS TO BEFORE DROP RENAME TO FACTURAS2;
	PUEDES PURGAR DE LA PAPELERA TABLAS CONCRETAS CON PURGE TABLE NOSE1122;
	
	// indexar por campor letra, hasta la f en una , hasta la p, hasta la z
	CREATE TABLE CATEGORIAS
	(NOMBRE VARCHAR2,
	CATEGORIA VARCHAR2)
	PARTITION BY RANGE (CATEGORIA)
	(PARTITION PRIMER_GRUPO VALUES LESS THAN ('F'),
	PARTITION SEGUNDO_GRUPO VALUES LESS THAN ('P'),
	PARTITION TERCER_GRUPO ALUESS LESS THAN ('Z')
	);

### *22/01/24*

	CREATE TABLE MATERIAS
		(CODIGO NUMBER PRIMARY KEY,
		NOMBRE VARCHAR2(30),
		PROFESOR VARCHAR2(30)
		)
		PARTITION BY LIST (PROFESOR)  
			(PARTITION SANTI VALUES ('SANTI','santi','s') TABLESPACE USUDAW
			PARTITION MLUZ VALUES('MLUZ','mluz','ml') TABLESPACE USUDAW2,
			PARTITION PABLO VALUES ('PABLO','pablo','p') TABLESPACE USUDAW,
			PARTITION PENDIENTE VALUES (DEFAULT) TABLESPACE USUDAW
			);
	Se necesita poner default, como max value de rango para las que no esten en lo listado

	falta despues del recreo   C8 
### *25/01/24*
	ALTER TABLE NOSE1122 RENAME PARTITION UNO TO UNOO; -- RENOMBRAR         PARTICIONES

	- CON LAS PARTICIONES, PODEMOS BORRARLAS SIN BORRAR LA TABLA
	ALTER TABLE NOSE1122 DROP PARTITION DOS;
	- PODEMOS HACER UN TRUNCATE DE LA PARTICION 
	ALTER TABLE NOSE1122 TRUNCATE PARTITION DOS [DROP STORAGE O REUSE       STORAGE]
	NO TIENE TUPLAS AL TRUNCARSE, PERO EXISTE LA PARTICION

	ALTER TABLE NOSE1122 TRUNCATE PARTITION DOS REUSE STORAGE;
	
	AÑADIR PARTITION: EN RANGED SOLO POR EL FINAL.
	ALTER TABLE NOSE1122 ADD PARTITION CUATRO VALUES LESS THAN (50);
	NO DEJA PORQUE DEBE TENER UN ORDEN MAYOR QUE EL DE LA ULTIMA PARTICION

	SI HAY NO UN MAXVALUE NO VA A DEJAR, NO HAY MAYOR QUE MAXVALUE

	EL BY HASH LIMITA, NO SE PUEDE HACER UN MERGER

	ALTER TABLE NOSE1122 MERGE PARTITIONS UNO,DOS INTO 
	PARTITION DOS;
	tiene que ir a la mayor, para que se siga cumpliendo el LESS THAN
	PRIMERO LA DE LIMITE INFERIOR (UNO,DOS)

	ALTER TABLE NOSE1122 EXCHANGE PARTITION TRES WITH TABLE NOSE1122BIS;
	hace drop storage

	

	SELECT NUMTOYMINTERVAL(1,'YEAR') FROM DUAL; SOLO YEARMONTH EN YM
	SELECT NUMTODSINTERVAL(1,'DAY') FROM DUAL; INTERVALOS DE DIA 
		DAY, HOUR,MINUTE,SECOND
PARTICIONAMIENTO POR INTERVALOS

	FUNCIONA CON FECHA Y CON NUMEROS, CON VARCHAR2  ¡¡¡ NO !!
	
### *29/01/24*
CREATE TABLE MUTANTE
 (COD NUMBE CONSTRAINT C_MUTANTE CHECK (COD>=1),
 NOM VARCHAR2(30),
 CONSTRAINT PK_MUTANTE 
            PRIMARY KEY(COD)
 )
 TABLESPACE USUDAW2
 PARTITION BY RANGE(COD)
           INTERVAL (10)
           (PARTITION PARI1 VALUES LESS THAN(11)
           );
           
 INSERT INTO MUTANTE VALUES(1,'UNO');
   
   
-- AL HECER UN ALTER TABLE MERGE PARTITION, EL INDICE SE QUEDA UNUSABLE, HAY QUE REINDEXAR.

ALTER INDEX PK_MUTANTE REBUILD;


CREATE TABLE PERSONAS 
 (DNI VARCHAR2(10),
 NOM VARCHAR2(30),
 GENERO CHAR(1),
 CODIGO NUMBER,
 CONSTRAINT PK_PERSONAS 
   PRIMARY KEY(DNI)
  )
  TABLESPACE USUDAW2
  PARTITION BY RANGE(CODIGO)
  SUBPARTITION BY HASH(GENERO)
   SUBPARTITION TEMPLATE  -- IMPORTANTE EL TEMPLATE
   (SUBPARTITION HOMBRES,
   SUBPARTITION MUJERES
   )
   (PARTITION UNO VALUES LESS THAN (2),
   PARTITION DOS VALUES LESS THAN (3),
   PARTITION TRES VALUES LESS THAN (4),
   PARTITION OTROS VALUES LESS THAN (MAXVALUE)
   );
   
   SELECT * FROM PERSONAS;
   DROP TABLE PERSONAS  CASCADE CONSTRAINT PURGE;


Falta el 01/02

### *05/02/24*
	Probando si funciona particionamiento primario de un tipo, y un subparticionamiento by list.
	STORAGE  -- a cualquiera d elas tablas, antes o despues de subpartition
		(INITIAL 128K
		NEXT 256 K 
		PCTINCREASE 50
		MINEXTENTS 2
		MAXEXTENTS 10
		);
*INDICES*
	CREATE [UNIQUE] INDEX NOMBRE
		ON TABLA (A1 [ASC/DESC], [A2 ASC/DESC])
		[TABLESPACE ......]
		[STORACE (.......) ];
		
*Tablas temporales*
no gastan espacio en disco, usan segmentos de memoria privada de cada usuario
se pueden usar en programas



### *09/02/24*
	 ON COMMIT PRESEVE ROWS - AUNQUE COMITEMOS SE MANTIENEN
	 SE DESTRUYE CUANDO SE CIERRA LA SESION
	GENERAN INFORMACION DE ROLLBACK 
	ROLLBACK WORK; 
	
TABLAS EXTERNAS 
	ni constraints, ni tablespaces, ni storage, ni atributos virtuales (estos en las temp tampoco)
	C13 tiene el proceso de creacion (ROL_DAW-> SCOTT o el nombre de user)

COMPRESIÓN DE TABLAS
	dos sistemas:
		-basica (comprime menos) y limitacion de no poder borrar atributos(columnas)
		-avanzada: no hay limitacion de las columnas
	se puede hacer a nivel de tablespace, a nivel de particion, subparticion y a nivel de tabla

### *12/02/24*
	IMPORTA DONDE PONES EL COMPRESS FOR, DENTOR DE UNA PARTICION, SOLO DONDE LA PONGAS LA TIENE,
	SI ES FUERA TODAS 

	NOCOMPRESS PARA DECLARAR QUE NO SE COMPRIMA 
	SI DECLARAS FUERA UN TIPO DE COMPRESION, TODAS LAS PARTICIONES SE COMPRIMEN ASÍ, SALVO QUE 
	ESPECIFICES EN ELLAS UNA DIFERENTE, Y ENTONCES ESA TIENE PRIORIDAD.

	ENCRIPTACIÓN 

		CREATE TABLE OTROEMPLE (DNI NUMBER, NOMBRE VA
  lo de abajo para una cosa rara de santi  sqlnet.ora
	CREATE TABLE OTROEMPLE (DNI NUMBER, NOMBRE VA says:ENCRYPTION_WALLET_LOCATION= (SOURCE= (METHOD=FILE) (METHOD_DATA= (DIRECTORY=C:\app\alumno\product\11.1.0\db_1\PARAWALLET) ) )
![[Pasted image 20240212182028.png]] aqui crear PARAWALLET (CARPETA)
![[Pasted image 20240212181804.png]]
![[Pasted image 20240212182141.png]]
![[Pasted image 20240212182305.png]]

### *16/02/24* 
	VISTAS
	CREATE OR REPLACE VIEW VISTA2 
		AS SELECT APELLIDO,LOC
			FROM EMPLE E 
				INNER JOIN DEPART D
					ON E.DEPT_NO=D.DEPT_NO;
	Diccionario de datos: 
	SELECT * FROM USER_VIEWS;
		FROM + TABLA (O VISTA O SINONIMO)
	SELECT * FROM VISTA2; NO GASTA ESPACIO 
	SELECT * FROM USER_OBJECTS (GUARDA DATOS DE DISTINTOS OBJETOS)
		WHERE USER_OBJECTS.OBJECT_TYPE='VIEW';
	CUANDO ES INVALIDA UNA LISTA? SE BASA EN UNAS TABLAS, SI BORRAS LAS TABLAS, LA VISTA QUEDA INVALIDA
	NO SE PUEDEN CREAR VISTAS BASADAS EN UN OBJETO NO DE NUESTRO USUARIO

	CREATE OR REPLACE VISTA3
		AS SELECT DEPT_NO,ROUND(AVG(SALARIO),2) AS MEDIA  // NECESITA EL ALIAS O ESPECIFICAR LAS COLUMNAS? 
			FROM EMPLE
			GROUP BY DEPT_NO;
	DROP TABLE EMPLE; LA VISTA INVALIDA.
	FLASHBLACK TABLE EMPLE TO BEFORE DROP; -> SIGUE INVALIDA HASTA QUE SE USE

	AL BORRAR UNA TABLA, SE BORRAN LAS CONSTRAINT, LOS INDICES Y LOS DERECHOS, LO QUE NO SE BORRAN SON LAS VISTAS (SE MARCAN COMO INVALIDAS, SINONIMOS, PROCEDIMIENTOS, FUNCIONES, PACK,TYPE...)

	EL MANTENIMIENTO DE LAS TABLAS BASE A TRAVÉS DE LAS VISTAS 

	CREATE OR REPLACE VIEW VISTA4
		AS SELECT APELLIDO,SALARIO
			FROM EMPLE 
			WHERE DEPT_NO IN (10,20);
	SELECT * FROM VISTA4;
	SE PUEDE BORRAR A SANCHEZ A TRAVES DE LA TABLA, SE PUEDE HACER A TRAVES DE LA VISTA??
	VEAMOS: 
	DELETE FROM VISTA4 
		WHERE APELLIDO="SÁNCHEZ";  EN EMPLE TAMPOCO ESTÁ
	DELETE FROM VISTA4 
		WHERE APELLIDO="NEGRO"; NO SE BORRA, PORQUE LA VISTA ES DEL DEPT_NO 10,20 , NEGRO ES DEL 30 
	UPDATE VISTA4 
		SET SALARIO = SALARIO + 1; SIN WHERE ES A TODOS (10,20); VISTA4 SE ACTUALIZA, EMPLE SOLO LOS DE (10,20) +1
	LO MISMO CON LOS ATRIUTOS, ESTO ES APELLIDO,SALARIO EN LA CREACION DE LA VISTA, OTRA COSA NO SE PUEDE MODIFICAR

	SI LA VISTA SE BASA EN UN JOIN, NO PODEMOS BORRAR, NI ACTUALIZAR NI INSERTAR A TRAVES DE LA VISTA
	TAMPOCO SE PUEDE BORRAR,ACTUALIZAR,INSERTAR SI LA VISTA TIENE FUNCIONES DE GRUPO (AVG, DISTINCT,GROUP BY)

	ACTUALIZACION: 
		CREATE OR REPLACE VIEW VISTA6
			AS SELECT APELLIDO,SALARIO,SALARIO*12 ANUAL
			FROM EMPLE;
		¿PUEDO BORRAR A TRAVES DE LA VISTA? 
		
		DELETE FROM VISTA6   -- SI , NADA DE GRUPO, NADA DE JOIN
			WHERE APELLIDO='SÁNCHEZ';

		UPDATE VISTA6
			SET SALARIO= SALARIO+1;  -- DEJA SALARIO
		UPDATE VISTA6
			SET ANUAL= ANUAL + 1; -- NO DEJA 
	INSERCIONES
		SI EN LA DEFINICION DE LA VISTA NO TENGO LOS CAMPOS OBLIGATORIOS , NO NOS VA A DEJAR

		CREATE OR REPLACE VIEW VISTA6 
			AS SELECT APELLIDO, SALARIO
				FROM EMPLE; NO SE VA A PODER INSERTAR PORQUE DEPT_NO Y EMP_NO SON OBLIGATORIOS
		INSERT INTO VISTA6 VALUES('PEPE',22);--NO
		CREATE OR REPLACE VIEW VISTA6 
					AS SELECT APELLIDO, SALARIO,DEPT_NO,EMP_NO
						FROM EMPLE;
		INSERT INTO VISTA6(APELLIDO,DEPT_NO,EMP_NO) VALUES ('NOE',22,222);  -- SI 

	HAY UNA VISTA DEL DICCIONARIO DE DATOS PARA SABER SI SE PUEDEN UPDATEAR, DELETEAR, INSERTAR
		SELECT * FROM USER_UPDATABLE_COLUMNS
			WHERE TABLE_NAME LIKE 'VISTA%';
	CREATE OR REPLACE VIEW VISTA1
		AS SELECT APELLIDO,SALARIO
			FROM EMPLE
				WITH READ ONLY;


	CAMBIAR DE NOMBRE A OBJETOS: (TABLAS Y VISTAS)
		RENAME (NOMBRE DE TABLA,VISTA O SINONIMO)
		RENAME EMPLE TO EMPLENUEO;
	SINONIMOS: 
		-PRIVADOS : EL QUE LOS CREA (1 USUARIO) LOS USA
		-PUBLICOS: USADOS POR TODOS (LOS CREA UNO CON PRIVILEGIO)

		UTILIDAD: 
		CFREATE TABLE NOSEQPONER 
		(X NUMBER); Y NOS DA DERECHOS SANTI A ESA TABLA DE CONSULTA (GRANT SELECT ON NOSEQPONER TO ...);
		PARA NO TENER QUE PONER SELECT * FROM PROFE.NOSEQPONER; EL PROFE.
		CREAMOS UN SINONIMO:
			CREATE SYNONIM NOSEQPONERSINONIMO FOR PROFE.NOSEQPONER; (PRIVILEGIOS INSUFICIENTES!)
			GRANT CREATE SYNONIM TO ROL_DAW;
		SELECT * FROM NOSEQPONERSINONIMO;

		PARA LOS PUBLICOS:
			CREATE PUBLIC SYNONIM NOSEQPONERSINONIMOPUBLICO FOR PROFE.NOSEQPONER;
		SELECT * FORM USER_SYNONIMS;
		DROP PUBLIC SYNONIM.... DROP SYNONIM...
### *19/02/24*
	VISTAS MATERIALIZADAS
		Gastan extend
		No va a cambiar de datos conforme cambie la original
		Hay que tener el derecho:
			GRANT CREATE MATERIALIZED VIEW
		TIPOS:
		1. CONSTRUCCION
			1.1 CONSTRUCCION INMEDIATA
			1.2 CONSTRUCCION DIFERIDA
			
		2. ACTUALIZACION
			2.1 ACTUALIZACION AL COMMITAR
			2.2 A DEMANDA
			2.3 POR TIEMPO
		3. INMUTABLES 
		4. COMPLETAS
		5. INCREMENTALES

	SINTAXIS:
	
		CREATE MATERIALIZED VIEW VISTAMA1
			BUILD IMMEDIATE
			NEVER REFRESH
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;
	DICCIONARIO DE DATOS:
		SELECT * FROM USER_MVIEWS;

	La equivalente de la de arriba no materializada:

		CREATE  VIEW VISTANOMA1
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;

	Los extent de las dos: SELECT * FROM USER_SEGMENTS WHERE SEGMENT_NAME IN('VISTAMA1','VISTANOMA1');
	La materializada es NEVER REFRESH así qe si hacemos INSERT INTO EMPLE(EMP_NO,DEPT_NO,APELLIDO,SALARIO) VALUES (2222,20,'NOSE',3434); La VISTANOMA1 la tiene, la materializada es un snapshot en un momento, no se actualiza con NEVER REFRESH.

	DROP MATERIALIZED VIEW VISTAMA1; NO SE GUARDAN EN LA PAPELERA 

	CREATE MATERIALIZED VIEW VISTAMA1
			BUILD DEFERRED -- LAS VISTAS SE POSTERGA SU INCORPORACIÓN  HASTA REFRESH -> AQUI SIEMPRE VACIA 
			NEVER REFRESH
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;
						
	CREATE MATERIALIZED VIEW VISTAMA1  -- CASCA, PEQUEÑO BUG POR LA MANERA DEL JOIN
			BUILD IMMEDIATE
			REFRESH COMPLETE ON COMMIT
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;
						
	CREATE MATERIALIZED VIEW VISTAMA1 -- ASI DEJA 
			BUILD IMMEDIATE -- NADA MAS CREARLA, SE CARGA DE DATOS
			REFRESH COMPLETE ON COMMIT -- SE ACTUALIZAN TODAS LAS TUPLAS CUANDO SE HAGA COMMIT
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E, DEPART D 
					WHERE E.DEPT_NO = D.DEPT_NO;
					
	CREATE MATERIALIZED VIEW VISTAMA1
			BUILD IMMEDIATE -- NADA MAS CREARLA, SE CARGA DE DATOS
			REFRESH COMPLETE ON DEMMAND -- CUANDO YO QUIERA, CON UN PROCEDIMIENTO
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;
	BEGIN
		DBMS_MVIEW.REFRESH('VISTAMA1'); -- AL EJECUTARLO SE ACTUALIZA
		
	CREATE MATERIALIZED VIEW VISTAMA1
			BUILD DEFERRED -- INICIALMENTE NO TIENE DATOS -> TIENE DATOS ON DEMMAND |
			REFRESH COMPLETE ON DEMMAND -- CUANDO YO QUIERA, CON UN PROCEDIMIENTO  <|
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;
						
	CREATE MATERIALIZED VIEW VISTAMA1
			BUILD DEFERRED -- SE LLENA CUANDO HAGA COMMIT
			REFRESH COMPLETE ON COMMIT -- 
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E, DEPART D 
					WHERE E.DEPT_NO = D.DEPT_NO;

	CREATE MATERIALIZED VIEW VISTAMA1 -- CASCA
			BUILD IMMEDIATE -- SE LLENA CUANDO HAGA COMMIT
			REFRESH COMPLETE ON COMMIT -- 
			AS SELECT APELLIDO,SALARIO,COMISION
				FROM EMPLE;
	
	CREATE MATERIALIZED VIEW VISTAMA1
			BUILD DEFERRED -- INICIALMENTE NO TIENE DATOS -> TIENE DATOS ON DEMMAND |
			REFRESH COMPLETE START WITH SYSDATE + (20/(24*60*60)) -- solo 1 actualizacion por el start with (recurrente? la siguiente)
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;
						
		CREATE MATERIALIZED VIEW VISTAMA1
			BUILD DEFERRED -- INICIALMENTE NO TIENE DATOS -> TIENE DATOS ON DEMMAND |
			REFRESH COMPLETE START WITH SYSDATE + (20/(24*60*60)) NEXT SYSDATE +  (20/(24*60*60)) -- cada 20 segundos
			AS SELECT EMP_NO,APELLIDO,SALARIO,COMISION,LOC
				FROM EMPLE E 
					INNER JOIN DEPART D
						ON E.DEPT_NO=D.DEPT_NO;
	INCREMENTALES (NECESITAMOS UN FICHERO LOG PARA IMPLEMENTAR SOLO LAS ACTUALIZACIONES)
		NI CON JOIN, GROUP BY, DISTINCT... FUCNIONES DE GRUPO
		(NO HAY CREATE OR REPLACE MATERIALIZED VIEW... EN VIEW SI)
		
	CREATE MATERIALIZED VIEW INCREMENTAL (NUM,APE,TOTAL) -- NOMBRE DE LOS CAMPOS CUSTOM
		BUILD IMMEDIATE 
		REFRESH FAST ON COMMIT
		AS SELECT EMP_NO,APELLIDO,SALARIO + NVL(COMISION,0) AS TOTAL
			FROM EMPLE
			ORDER BY TOTAL; -- NECESITA UN LOG (QUE NECESITA A SU VEZ UNA PK, PORQUE
			LOS CREATE AS SELECT NO COPIAN LAS CONSTRAINTS)
		ALTER TABLE EMPLE ADD CONSTRAINT PK_EMPLE PRIMARY KEY (EMP_NO);
		AHORA NECESITA UN LOG DONDE APUNTAR LAS NOVEDADES
		
	CREATE MATERIALIZED VIEW LOG ON EMPLE WITH PRIMARY KEY; -- SOLO 1 LOG POR TABLA 
	SI 2 VISTAS MATERIALIZADAS UTILIZAN EMPLE, SOLO HACE FALTA EMPLE
		COMO BORRAR UN LOG: DROP MATERIALIZED VIEW LOG ON EMPLE; 
	Diccionario de datos para ver logs: SELECT * FROM USER_MVIEWS_LOGS;
		SELECT * FROM MLOG$_EMPLE; TABLA DE NOTACIONES DE CAMBIOS (PENDIENTES)
	
	DROP MATERIALIZED VIEW INCREMENTAL; -- NO BORRA EL LOG 
	
	CREATE MATERIALIZED VIEW INCREMENTAL (APE,TOTAL) -- AL CREAR EL LOG CON PRIMARY KEY NO        PUEDES QUITAR EL CAMPO PK EN LA SELECT
		BUILD IMMEDIATE 
		REFRESH FAST ON COMMIT
		AS SELECT APELLIDO,SALARIO + NVL(COMISION,0) AS TOTAL
			FROM EMPLE
			ORDER BY TOTAL;
			
	CREATE MATERIALIZED VIEW LOG ON EMPLE WITH ROWID; (O PRIMARY KEY O ROWID)
			CON PRIMARY KEY LA TABLA NECESITA PRIMARY KEY Y
			LA SELECT EN LA QUE SE BASA LA VISTA MATERIALIZADA TIENE QUE TENER LA PK
			ROWID EL LOG LA SELECT NO TIENE POR QUE TENER LA PK, PERO HAY QUE AÑADIR WITH                 ROWID
			
	CREATE MATERIALIZED VIEW INCREMENTAL (APE,TOTAL) -- 
		BUILD IMMEDIATE 
		REFRESH FAST ON COMMIT
		WITH ROWID -- NECESITA PARA QUE NO CASQUE
		AS SELECT APELLIDO,SALARIO + NVL(COMISION,0) AS TOTAL
			FROM EMPLE
			ORDER BY TOTAL; 
			
	SE PUEDE HACE JOIN + FAST REFRESH? REFRESH FAST FUNCIONA ON DEMMAND, COMMIT, START, NEXT...

### *26/02/24*

	PRIMER EJEMPLO,  DE VISTA10 CON INMEDIATE FUNCIONA, CON DEFERRED NO FUNCIONA EL FAST REFRESH 
	(HA JUNTAODO EL EJEMPLO EN EL DOCUMENTO)
	CON DEFERRED FAST REFRESH TIENES LAS NOVEDADES PERO NO TIENES LA BASE

	NO SE PUEDE HACER FAST CON JOIN O FUNCIONES DE GRUPO (EN TEORIA)

	EN VEZ DE FAST, REFRESH FORCE ON DEMAND 

	REFRESH COMPLETE , FAST Y FORCE

	FORCE ALMACENA LOS CAMBIOS, CUANDO ACTUALIZA HACE EL HISTORICOS POR ORDEN


	---CONSULTAS EN PARALELO ----
	
	CADA CORE AGILIZE LA CONSULTA
	A NIVEL DE SESION, TABLA, SELECT
	
	SELECT * FROM EMPLE; - SIN PARALELAJE

	SELECT /*+ PARALLEL (4) */ * FROM EMPLE22;
	SELECT /*+ PARALLEL (E,4) */ * FROM EMPLE22;

	SELECT APELLIDO,LOC
		FROM EMPLE22 E 
			INNER JOIN DEPART22 D 
				ON E.DEPT_NO=D.DEPT_NO; -- LA NORMAL
				
	SELECT /*+ PARALLEL (E,4) */ /*+ PARALLEL (D,4) */APELLIDO,LOC
		FROM EMPLE22 E 
			INNER JOIN DEPART22 D 
				ON E.DEPT_NO=D.DEPT_NO;
	CON SESIONES
		ALTER SESSION ENABLE PARALLEL DML;
		ALTER SESSION ENABLE PARALLEL DDL;
		ALTER SESSION ENABLE PARALLEL QUERY;
		
		ALTER SESSION FORCE PARALLEL DML PARALELL 4;-- PARA ESTABLECERLO PERSONALIZADO CUANTO QUIERES DE PARAL.
		
	POR TABLAS
		ALTER TABLE EMPLE22 PARALLEL 8;
		ALTER TABLE EMPLE22 NOPARALLEL;
		-- ESA TABLA SIEMPRE
		
---PLAN DE EJECUCION---

	EXPLAIN PLAN FOR... C18


### *29/02/24* falta

### *01/03/24*
	crear usuarios 
		sintaxis: CREATE USER nombreUSUARIO
			IDENTIFIED BY claveUSUARIO
			[ (opcional)
			DEFAULT TABLESPACE nombreTABLESPACE
			TEMPORARY TABLESPACE nombreTAB_T
			QUOTA ON , ... . .. .
			PROFILE NOMBRE
			];
		hay que tener el derecho CREATE USERS - > GRANT CREATE USERS TO
	derechos:
		2 tipos:
			- DERECHOS DEL SISTEMA (create user, flashback)
				GRANT D1,D2.....
					TO PUBLIC 
						ROL
						USUARIO
						[WITH ADMIN OPTION]; admin option recibe derecho y ademas puede cederselo a un 3º
				SELECT COUNT (*) FROM DBA_SYS_PRIVS;
				DESPUES DE CREARLO HAY QUE DARLE DERECHO : GRANT CREATE SESSION TO NUEVO;
				(PARA DAR ESE DERECHO HAY QUE HACER GRANT CREATE SESSION TO PROFE WITH ADMIN OPTION PARA PODER DARLO A NUEVO);
			NUEVO (C20)  NO TIENE QUOTA SUFICIENTE, PARA DARLE MAS QUOTA
			UN USUARIO POR DEFECTO SOLO PUEDE CAMBIARSE A SI MISMO LA CLAVE
				ALTER USER NUEVO 
					QUOTA 70K ON USUDAW;
			(QUITAR DERECHOS -> REVOKE .... FROM )
			DROP USER NUEVO CASCADE; (cascade porque tiene que vaciarse el objeto)
			
			- DERECHOS SOBRE LOS OBJETOS - (EQUIVALENTE A WITH ADMIN OPTION 
										 ES WITH GRANT OPTION) !!!
					INSERT, UPDATE, DELETE
						INDEX REFERENCES EXECUTE ALTER  -- REFERENCES A ROL NO
				CREATE TALBE NOSE123 ( X NUMBER PRIMARY KEY, Y NUMBER);
						GRANT SELECT, INSERT ON NOSE123 TO ROL_DAW;
				GRANTOR EL QUE DA , GRANTEE EL QUE RECIBE

			QUOTA X K/M o UNILIMITED

SELECT USERNAME,TABLESPACE_NAME, TRUNC((BYTES/MAX_BYTES)*100,2) AS PORCENTAJE FROM DBA_TS_QUOTAS 
       WHERE USERNAME LIKE 'ALU%';

### *04/03/24*
	ROLES 
		1º crea un usuario (scott): CREATE USER NUEVO
									IDENTIFIED BY nuevo 
									DEFAULT TABLESPACE USUDAW
									TEMPORARY TABLESPACE TEMP
									QUOTA 1000K ON USUDAW; -- NO PRIV. ASI QUE GRANT
								GRANT CREATE USER TO SCOTT;
								
								CREATE ROLE NUEVOROL; -- NO PRIV. GRANT
								GRANT CREATE ROLE TO PROFE;
								
								GRANT NUEVOROL TO NUEVO;
								GRANT NUEVOROL TO NUEVO2; (SIGUE SIN PODER ENTRAR PORQUE NO TIENEN DCHOS)
								
								GRANT CREATE SESSION, CREATE TABLE, CREATE VIEW
									TO NUEVOROL; NO LOS TIENE PROFE CON ADMIN OPTION, NECESITA QUE SYS SE LO HAGA
								nuevo no puede mirar profe.emple; como se le da derechos?
						desde profe al rol (dchos sobre objetos): 
						
								GRANT SELECT ON EMPLE TO NUEVOROL;
							
						CREATE TABLE XX (Y NUMBER, YY NUMBER); -- CUALQUIERA
						ahora una vista:
							CREATE VIEW VISTAXX AS SELECT * FROM XX;
						no puede crear vistas a partir de objetos que se le ha concecido acceso via rol!!
						puede hacer visstas, puede select * from profe.emple, pero  ^^^^^^^^^^^^^^^^^^^
						
						
						slect * from ROLE_ROLE_PRIVS; -- roles asignados roles
							CONNECT y RESOURCE
						SELECT * FROM SESSION_ROLES;
						SELECT * FROM USER_ROLE_PRIVS; roles de los usuarios (asignados directos)
							
						SELECT * FROM DBA_ROLE_PRIVS 
							WHERE GRANTEE IN ('PROFE');
							
						SELECT * FROM DBA_SYS_PRIVS
							WHERE GRANTEE IN ('CONNECT');
						
						privilegios del sistema:
							SELECT * FROM DBA_SYS_PRIVS
								WHERE GRANTEE IN('NUEVOROL'); -- DCHOS DEL SISTEMA
						privilegios de objetos:
							SELECT * FROM DBA_TABS_PRIVS
								WHERE GRANTEE IN('NUEVOROL');
			PERFILES 
					https://jorgesanchez.net/manuales/abd/control-usuarios-oracle.html
					
					
		SELECT * FROM DBA_PROFILES
			WHERE PROFILE = 'DEFAULT';
			
		SELECT * FROM DBA_USERS
			WHERE USERNAME IN ('NUEVO');
			
	cuando creamos un usuario si no indicamos
	profiles, se le asigna el profiles->default
	
	creando un profile: (se necesita derecho)
	
	CREATE PROFILE MIPROFILE LIMIT
	SESSIONS_PER_USER 1
	FAILEDLOGIN_ATTEMPS 1
	IDLE_TIME UNLIMITED; (MINUTOS)
	los que no especifigues toman valor del 
	profile default
	
	ALTER PROFILE MIPROFILE LMIT
	SESSIONS_PER_USER 2;
	
	Se puede asignar a un usuario un profile:
		1. cuando lo creas (el usuario con profile)
		2. con alter user profile (nombreProfile);
		   
	ALTER SYSTEM SET RESOURCE_LIMIT = TRUE;
	esto activa los profiles 
	
		ALTER USER NUEVO3 ACCOUNT UNLOCK;
	
		personalizando el control de la clave usuario
		(como sys) : 
		 CREATE OR REPLACE FUNCTION VALIDAR
			 (USUARIO,CLAVENUEVA,CLAVEANTIGUA);
		CREATE OR REPLACE FUNCTION VALIDAR
			(USU IN VARCHAR2, NUE IN VARCHAR2,
			VIE IN VARCHAR2)
			RETURN BOOLEAN
			AS
				SW BOOLEAN;
				BEGIN
					IF LENGTH(NUE)<4 OR
					NUE=VIE THEN
					SW:=FALSE;
					ELSE 
					SW:=TRUE;
					END IF;
					RETURN SW;
				END VALIDAR;
### *07/03/24*
	SELECT * FROM DBA_PROFILES; 
	ALTER PROFILE (NECESITA PRIV) LIMIT
		PASSWORD_VERIFY_FUNCTION VALIDAR; (LLAMA A LA FUNCION VALIDAR)
	ALTER SYSTEM SET RESOURCE_LIMIT = TRUE; - LO VIMOS , ACTIVAR ESO PARA PROFILES 
	ALTER USER NUEVO3 PROFILE MIPROFILE;
	DESDE NUEVO3 SESION INICIADA; 
		ALTER USER NUEVO3 IDENTIFIED BY noseqponer REPLACE nuevo;
			nuevo es la anterior
			
	******** tablespaces *********
		CREATE TABLESPACE NUEVO
			DATAFILE 'C:\app\alumno\oradata\orcl\NUEVO01.DBF'
				SIZE (EN K O M) 100 K REUSE (SI YA EXISTE, SOBREESCRIBE),
				'C:\app\alumno\oradata\orcl\NUEVO02.DBF'
				SIZE 100 K REUSE 
			DEFAULT STORAGE
				(INITIAL 64 K 
				NEXT 64 K
				PCTINCREASE 40
				MINEXTENTS 2
				)
			ONLINE; (CREA -> DISPONIBLE, ONLINE POR DEFECTO)
			OFFLINE LO PONE NO DISPONIBLE
			
		SELECT * FROM DBA_TABLESPACES; (COMO SYS/SYSTEM, DCHOS)
		SELECT * FROM DBA_DATA_FILES; LOS FICHEROS DE LOS TABLESPACES

		ALTER USER NUEVO3
			QUOTA 300 K ON NUEVO;

		COMO AMPLIAR UN TABLESPACE:
			ALTER TABLESPACE NUEVO
				ADD DATAFILE 'C:\app\alumno\oradata\orcl\NUEVO03.DBF'
					SIZE 400 K REUSE;
		AMPLIACION DE QUOTA A USUARIO (TENIA 300 K):
			ALTE RUSER NUEVO3 
				QUOTA 1 M ON NUEVO;
				
		PODEMOS HACER QUE SE AUTOEXTENDA EL TABLESPACE:
		LE DAMOS OTRO FICHERO PERO AUTOEXTEND (lo podemos decir al crearlo tambien!)
			ALTER TABLESPACE NUEVO
				ADD DATAFILE 'C:\app\alumno\oradata\orcl\NUEVO04.DBF'
					SIZE 400 K REUSE
					AUTOEXTEND ON NEXT 1 M MAXSIZE [UNLIMITED]/[5 M];
					
		PARAR UN TABLESPACE:
			ALTER TABLESPACE NUEVO OFFLINE;
		
		BORRAR TABLESPACE: 
			-1º DROP
			-2º DEL S.O
		DROP TABLESPACE NUEVO INCLUDING CONTENTS;
		
	TABLESPACES TEMPORALES (CACHE)
	
		CREATE TEMPORARY TABLESPACE TEMPORAL
			TEMPFILE 'C:\app\alumno\oradata\orcl\TEMPORAL.DBF'
					SIZE 1000 K; CASCA , (MINIMO ES 1100 K)
					
		CREATE TABLE ONSE (X NUMBER) 
			TABLESPACE TEMPORAL; -- NO TABLA NORMALES EN TABLESPACES TEMPORALES
			
		CREATE GLOBAL TEMPORARY TABLE NOSE (X NUMBER) 
			TABLESPACE TEMPORAL; ASÍ SI 
			
	**** VARRAY ***** (COLECCION)
		CREATE OR REPLACE TYPE TELEFONOS AS VARRAY(3) OF NUMBER(9); 
		-- COLECCION DE 3 COSAS, DE NUMBEROS DE MAX 9 DIGITOS
			SELECT * FROM USER_TYPES WHERE TYPE_NAME = 'TELEFONOS'; - LA COLECCION
			SELECT *NFROM USER_VARRAYS WHERE TYPE_NAME ='TELEFONOS'; - SU USO EN DATOS
		
		CREATE TABLE PERSONAS
		(DNI NUMBER PRIMARY KEY,
		NOM VARCHAR2(20),
		SAL NUMBER,
		COMI NUMBER,
		TOTAL GENERATED ALWAYS AS (SAL+ COMI) VIRTUAL,
		TEL TELEFONOS);
		
	INSERT INTO PERSONAS(DNI,NO,SAL,COMI,TEL) VALUES (1,'UNO',3,2,TELEFONOS(111,222,333));
	INSERT INTO PERSONAS(DNI,NO,SAL,COMI,TEL) VALUES (2,'DOS',4,2,TELEFONOS(111));
	INSERT INTO PERSONAS(DNI,NO,SAL,COMI,TEL) VALUES (3,'TRES',3,6,TELEFONOS(222,333));
	
	SELECT NOM,DNI, P.TEL -> ALIAS NECESARIO PARA ACCEDER A LOS VALORES DE LA COLECCION
		FROM PERSONAS P  -> ALIAS NECESARIO PARA ACCEDER A LOS VALORES DE LA COLECCION
			WHERE 222 IN (SELECT * FROM TABLE(TEL));
		
		-- MIRAR LO DE LA CLASE DE SANTI  EN TABLA PONER NTABLE, TABLE(TEL) P PARA VEERLO
															P.COLUMN_VALUE

### *11/03/24*
	
# E- 3_APUNTES_LIMPIO
## E.1- VISTAS IMPORTANTES
		user_cons_columns,  
		user_tab_cols
		USER_CONSTRAINTS
		USER_EXTENTS
		USER_TABLES
		
		SELECT * FROM USER_TAB_PARTITIONS
			WHERE TABLE_NAME = 'DOCENTES';
   
		USER_IND_PARTITIONS;
		
		SELECT * FROM USER_SEGMENTS
		    WHERE SEGMENT_NAME='NOSE1122';
		    
		SELECT * FROM USER_EXTENTS
		     WHERE SEGMENT_NAME='NOSE1122';   
		     
		 SELECT PARTITION_NAME,COUNT(*)
	        FROM USER_EXTENTS
		        WHERE SEGMENT_NAME = 'NOSE1122'
		        GROUP BY(PARTITION_NAME);
		        
		  USER_TAB_SUBPARTITIONS
		    
## E.2- TABLAS 
###  PRIMARY KEYS
				- PK conjuntas
					No se pueden hacer a nivel de atributo 	                   
			             CREATE TABLE NOSE11 
			             (
			             NUMF NUMBER PRIMARY KEY, 
			             CODP NUMBER PRIMARY KEY,
			             UNIDADES NUMBER
			             ) TABLESPACE USUDAW;
		             A nivel de tabla sí
				        CREATE TABLE NOSE11  
			             (
			             NUMF NUMBER, 
			             CODP NUMBER,
			             UNIDADES NUMBER,
			             CONSTRAINT PK_NOSE11 PRIMARY KEY(NUMF,CODP) 
			             ) TABLESPACE USUDAW;
			             
				- INITIALLY IMMEDIATE / INITIALLY DEFERRED
				  
### FOREIGN KEYS
			
			DROP TABLE PERSONAS CASCADE CONSTRAINT PURGE;
			ON DELETE CASCADE EN LA FK

						 CREATE TABLE DETALLES 
		       (UNIDADES NUMBER,
		       NUMF NUMBER CONSTRAINT FK_DETALLES_FACTURAS 
		                             REFERENCES FACTURAS(NUMF)
		                             ON DELETE CASCADE,
		
			   CREATE TABLE TITULARIDADES
			     (DNI VARCHAR2(20),
			      MATRICULA VARCHAR2(7)  CONSTRAINT C_COCHES2 CHECK (LENGTH(MATRICULA)=7),
			      FECHA DATE DEFAULT SYSDATE,
			      CONSTRAINT PK_TITULARIDADES PRIMARY KEY(DNI,MATRICULA),
			      CONSTRAINT FK_TITU_PERSONAS FOREIGN KEY(DNI) REFERENCES PERSONAS (DNI),
			      CONSTRAINT FK_TITU_CHOCHES FOREIGN KEY(MATRICULA) REFERENCES COCHES (MATRICULA)
			      );

### UNICIDAD, OBLIGATORIEDAD, DEFAULT
				CREATE TABLE EJEMPLO
				(CAMPO1 NUMBER NOT NULL,
				CAMPO2 NUMBER UNIQUE,
				CAMPO3 NUMBER DEFAULT 4 NOT NULL);
				
				DEFAULT *NO* ADMITE FUNCIONES DE SQL NI ATRIBUTOS DE  TABLA, NI CONSTRAINT
				SI ADMITE SYSDATE, USER,UID    ->  CHECK NO LAS PUEDE UTILIZAR 
				NO A NIVEL DE TABLA
				SI UN ATRIBUTO TIENE VARIAS RESTRICCIONES
				DEFAULT NO PUEDE SER LA ÚLTIMA RESTRICCION (SALVO QUE SEA LA ÚNICA)
				NOT NULL TAMPOCO PUEDE ESTAR A NIVEL DE TABLA 
					
				NOT NULL
	
### CHECK
			
		CORREO VARCHAR2(20) CONSTRAINT C_CORREO_NOSE11 CHECK(CORREO LIKE '%@%.%') CHECK ( CORREO LIKE '%_@_%._%' AND SUBSTR (CORREO, INSTR(CORREO,'.'+1) IN ('ES','COM','ORG'))),
		
### ATRIBUTOS VIRTUALES
			SE PUEDE HACER PK DE ATRIBUTOS VIRTUALES 
			SOLO PUEDE TENER: CHECK Y DEFAULT
			    
				Los atributos virtuales no gastan espacio en disco. Desventaja : hay que procesar la expresión para calcularlos. Estan disponibles 
				Tantos atributos virtuales como queramos, en cualquier orden. Lo normal es ponderlos al final 
					CREATE TABLE AOSKDOQWE
					(C TIPO.........,
					C2 TIPO..........,
					ATRIBVIRTUAL [TIPO DE DATO] GENERATED ALWAYS AS (EXPRESION SQL) 
### VIRTUAL
					) [ TABLESPACE ASDQWE ] ; 

		CREATE TABLE DESCRIPCION1 (
		           DNI NUMBER PRIMARY KEY,
		           DESCRIPCION VARCHAR2(20) CONSTRAINT C_DESC_DESCRIPCION1 CHECK(DESCRIPCION IN('MESA','SILLA','CUADRO')),
		           CODIGO VARCHAR2(50) GENERATED ALWAYS AS (
		              DECODE (DESCRIPCION,
		                 'MESA', 'CODIGO1-',
		                 'SILLA', 'CODIGO2-' ,
		                 'CUADRO', 'CODIGO3-',
		                 DESCRIPCION
		              ) || '-' ||  LENGTH(DESCRIPCION)
		           ) VIRTUAL
		        );
		1. OPERACIONES CON TABLAS
		   
			TRUNCATE TABLE -> borra los datos de la tabla
			-------------------FORMATO TRUNCATE
			
			TRUNCATE TABLE NOMBRE_TABLA [REUSE STORAGE / DROP STORAGE (DEFECTO)];     
			
			TRUNCATE TABLE NOSE3 REUSE STORAGE;
			
			
			SELECT * FROM USER_SEGMENTS
			  WHERE SEGMENT_NAME = 'NOSE3';
			  
			SELECT * FROM USER_EXTENTS
			  WHERE SEGMENT_NAME = 'NOSE3';  
						DROP TABLE ... ;
						DROP TABLE ... CASCADE CONSTRAINT; 
						DROP TABLE ... PURGE;
						FLASHBACK TABLE ... TO BEFORE DROP;
			
### COPIAR TABLAS

				CREATE TABLE NOSE223344 AS SELECT * FROM NOSE2233;    ç
				*no se copian las constraint que tengan nombre personalizado* 

	
				La copia no tiene por qué ser igual 
				
					CREATE TABLE EMPLEDOS
					AS SELECT EMP_NO,APELLIDO,SALARIO
					FROM EMPLE
					WHERE OFICIO='ANALISTA';

			 --CASCA, HAY QUE PONER ALIAS
			CREATE TABLE EMPLEDOS
			    AS SELECT EMP_NO,APELLIDO,NVL(SALARIO,0) + NVL(COMISION,0)
			       FROM EMPLE; 
			 
			 CREATE TABLE EMPLEDOS
			    AS SELECT EMP_NO,APELLIDO,NVL(SALARIO,0) + NVL(COMISION,0) AS TOTAL
			       FROM EMPLE;    
			       
			CREATE TABLE DEPARTAMENTO2(APELLIDO,OFICIO,LOCALIDAD)
	         AS SELECT APELLIDO,OFICIO,LOC
	            FROM EMPLE 
	                 NATURAL JOIN DEPART;****
						
### ALTER TABLES
			DROP TABLE PERSONAS CASCADE CONSTRAINT PURGE;
			
						ALTER TABLE PRUEBA 
				ADD XXX NUMBER, XXXX NUMBER);
			
				ALTER TABLE PRUEBA DROP(XXX,XXXX);
			
				ALTER TABLE PRUEBA MODIFY (X NUMBER(10)) ;   SIN DATOS TODO VA BIEN PERO CUANDO HAY DATOS...
			
				ALTER TABLE PRUEBA MODIFY (XX VARCHAR2(6)) ; SI EL DATO QUE HAY ESTA DENTRO DE LA DIMENSION, NP, SI NO, NO DEJA EN VARCHAR2
													     CON NUMBER DEJA AUMENTARLO, PERO NUNCA REDUCIRLO, AUNQUE EL NUMBER ENTRE
			
				SE PUEDEN AÑADIR RESTRICCIONES SI NO HAY DATOS, SIN PROBLEMA  , SI TIENE DATOS? DEPENDE DE SI LA VIOLAN O NO
			
				ALTER TABLE PPRUEBA MODIFY (X NUMBER (5) ) CONSTRAINT PK_PRUEBA
													PRIMARY KEY
													);
				ALTER TABLE PRUEBA MODIFY (X NUMBER(5) 
	            CONSTRAINT PK_PRUEBA
	             PRIMARY KEY
		         );
		         
		 ALTER TABLE PRUEBA DROP CONSTRAINT PK_PRUEBA;   
			ALTER TABLE PRUEBA ADD CONSTRAINT   PK_PRUEBA
			                       PRIMARY KEY(X);     
			           ALTER TABLE NOMBRE  ADD (COL TIPO [RESTRICCIONES], COL2 TIPO2 [RESTRICCIONES]........);
                    DROP (COL,COL..................);
                    MODIFY (COL TIPO [RESTRICCIONES], COL2 TIPO [RESTRICCIONES].....);
                    ADD CONSTRAINT NOMBRE RESTRICCION;
                    DROP CONSTRAINT  NOMBRE;
                    ENABLE CONSTRAINT NOMBRE;
                    DISABLE CONSTRAINT NOMBRE;
                
	         ALTER TABLE PRUEBA ENABLE CONSTRAINT PK_PRUEBA
	          EXCEPTIONS INTO FALLOS;  
	      
		ALTER TABLE nombre_tabla
		ADD (nombre_columna_virtual tipo_dato GENERATED ALWAYS AS (expresion) VIRTUAL);
	
		ALTER TABLE EMPLEADOS
		ADD CONSTRAINT chk_salario_total CHECK (SALARIO_TOTAL >= 0);
    
					ALTER TABLE TABLAV
					     ADD (INICIALES GENERATED ALWAYS AS 
					                     ( SUBSTR(NOM,1,1) ||'.'||
					                       SUBSTR(APE1,1,1) ||'.' ||
					                       SUBSTR(APE2,1,1) ||'.'
					                      ) VIRTUAL
					         );
					
### PAPELERA / FLASHBACK
	 SELECT * FROM USER_RECYCLEBIN;
	 PURGE USER_RECYCLEBIN;

	  FLASHBACK TABLE FACTURAS TO BEFORE DROP;
	  FLASHBACK TABLE FACTURAS TO BEFORE DROP RENAME TO FAC;

	Borrar la papelera de reciclaje del user:
			PURGE RECYCLEBIN;
	Borrar toda la papelera de reciclaje:
			PURGE DBA_RECYCLEBIN;
### TABLAS TEMPORALES
	-NO USAN EXTENTS DE DISCO
	- USAN SEGMENTOS DE MEMORIA PRIVADA DE CADA USUARIO
	-PUEDEN TENER CONSTRAINT
	-NO SE PUEDEN PARTICIONAR
	-NO SE PUEDEN COMPRIMIR
	-NO GENERAN INFORMACION DE REDOLOG
	-DOS TIPOS TABLAS TEMPORADES:
	       -NORMALES----> NO GENERA ROLLBACK
	                -----> AL COMITAR/ROLLBACK SE VACIA
	       -ON COMIT ----> SI GENERA INFORMACION DE ROLLBACK
	               --> AL CONCLUIR LA SESSION
	               
	-SE GUARDA LA ESTRUCTURA DE LA TABLA PERO NUNCA LOS DATOS
	-SE PUEDEN DAR DERECHOS A OTROS USUARIOS
	  PERO CADA USUARIO TIENE SU PROPIA COPIA DE LA TABLA
	-NO PUEDEN USAR TABLESPACE NI STORAGE  
	  
  
	 CREATE GLOBAL TEMPORARY TABLE NOMBRE 
	    (
	    ..............
	    ..............
	    ............
	    ) [ON COMMIT PRESERVE ROWS];  
  

		CREATE GLOBAL TEMPORARY TABLE
		        TEMPORAL
		        (X NUMBER CONSTRAINT PK_TEMPORARL 
		          PRIMARY KEY,
		         XX NUMBER
		         )
		         TABLESPACE USERS;  --CASCA
		  
		CREATE TABLE
		        NORMAL
		        (X NUMBER CONSTRAINT PK_NORMAL 
		          PRIMARY KEY,
		         XX NUMBER
		         );        
		 
		CREATE GLOBAL TEMPORARY TABLE
		        TEMPORAL
		        (X NUMBER CONSTRAINT PK_TEMPORARL 
		          PRIMARY KEY,
		         XX NUMBER
		         );
		
		CREATE GLOBAL TEMPORARY TABLE
		        TEMPORAL2
		        (X NUMBER CONSTRAINT PK_TEMPORARL2 
		          PRIMARY KEY,
		         XX NUMBER
		         ) ON COMMIT PRESERVE ROWS;         
		         
		 SELECT * FROM USER_TABLES WHERE TABLE_NAME IN ('TEMPORAL','NORMAL','TEMPORAL2');
		INSERT INTO TEMPORAL2  VALUES(1,1); 
		INSERT INTO TEMPORAL2  VALUES(2,1); 
		SELECT * FROM TEMPORAL2;
       
### TABLAS EXTERNAS
	ni constraints, ni tablespaces, ni storage, ni atributos virtuales (estos en las temp tampoco)
	- No hay transacciones, no podemos insertar, no podemos borrar, actualizar, …
	
	CREATE TABLE EXTERNA2
	   ( DEP NUMBER ,
	     SALDO NUMBER (12,2),
	     LOC VARCHAR2(20),
	     DNOMBRE VARCHAR2(30),
	     FECHA date
	   )
	  ORGANIZATION EXTERNAL
	   (
	    TYPE ORACLE_LOADER DEFAULT DIRECTORY RUTA   
	    ACCESS PARAMETERS ( RECORDS DELIMITED BY NEWLINE
	                        SKIP 0
	                        FIELDS TERMINATED BY ','
	                        MISSING FIELD VALUES ARE NULL
	                          ( DEP INTEGER EXTERNAL(2),
	                            SALDO FLOAT EXTERNAL(12),
	                            LOC CHAR(20),
	                            DNOMBRE CHAR(30),
	                            FECHA CHAR(10) DATE_FORMAT DATE MASK "dd-mm-yyyy"
	                          )  
	                      )
	
	    LOCATION ('DATOS2.TXT')
	    );
	    
	SELECT * FROM EXTERNA;    
	
	CREATE OR REPLACE DIRECTORY RUTA1 AS 'C:\app\alumno\FICHEROS';     
	SELECT SALDO,DEP FROM EXTERNA;
	DELETE FROM EXTERNA;
	GRANT ALL ON EXTERNA TO ROL_DAW;
	
	SELECT * FROM USER_SEGMENTS
	    WHERE SEGMENT_NAME IN ('EMPLE','EXTERNA','TEMPORAL1');
	    
	    
	 ALTER TABLE EXTERNA LOCATION('DATOS2.TXT','DATOS3.TXT');  


## E.3- Particiones

	ALTER TABLE NOSE1122 RENAME PARTITION UNO TO UNOO; -- RENOMBRAR         PARTICIONES
	ALTER TABLE NOSE1122 EXCHANGE PARTITION TRES WITH TABLE NOSE1122BIS;
							   SUBPARTITION

	   ALTER TABLE ALUMNO2 ADD PARTITION  CUATRO VALUES (9,10);  --CASCA     
	   CUANDO TIENES YA UNA POR DEFECTO 




		- CON LAS PARTICIONES, PODEMOS BORRARLAS SIN BORRAR LA TABLA
		ALTER TABLE NOSE1122 DROP PARTITION DOS;
		- PODEMOS HACER UN TRUNCATE DE LA PARTICION 
		ALTER TABLE NOSE1122 TRUNCATE PARTITION DOS [DROP STORAGE O REUSE       STORAGE]
		NO TIENE TUPLAS AL TRUNCARSE, PERO EXISTE LA PARTICION
	
		ALTER TABLE NOSE1122 TRUNCATE PARTITION DOS REUSE STORAGE;
		
		AÑADIR PARTITION: EN RANGED SOLO POR EL FINAL.
		ALTER TABLE NOSE1122 ADD PARTITION CUATRO VALUES LESS THAN (50);
		NO DEJA PORQUE DEBE TENER UN ORDEN MAYOR QUE EL DE LA ULTIMA PARTICION
	
		SI HAY NO UN MAXVALUE NO VA A DEJAR, NO HAY MAYOR QUE MAXVALUE

				SE PUEDEN ALMACENAR CADA UNA EN DIFERENTES TABLESPACES
				
			 SELECT * FROM ALUMNOS PARTITION(OTROS);   
				Mas rapido que una select
### RANGO
	CREATE TABLE FACTURAS (
	  NIF VARCHAR2(20),
	  NOMBRE VARCHAR2(100),
	  IMPORTE NUMBER,
	  FECHA DATE
	)
	PARTITION BY RANGE(FECHA) (
	  PARTITION PRIMER_TRIMESTRE VALUES LESS THAN (TO_DATE('1/ABRIL/2023','dd/MON/yyyy')) TABLESPACE USUDAW,
	  PARTITION SEGUNDO_TRIMESTRE VALUES LESS THAN (TO_DATE('1/JULIO/2023','dd/MON/yyyy')) TABLESPACE USUDAW2,
	  PARTITION TERCER_TRIMESTRE VALUES LESS THAN (TO_DATE('1/OCTUBRE/2023','dd/MON/yyyy')) TABLESPACE USUDAW,
	  PARTITION CUARTO_TRIMESTRE VALUES LESS THAN (TO_DATE('1/ENERO/2024','dd/MON/yyyy'))
	);


		// indexar por campor letra, hasta la f en una , hasta la p, hasta la z
			CREATE TABLE CATEGORIAS
			(NOMBRE VARCHAR2,
			CATEGORIA VARCHAR2)
			PARTITION BY RANGE (CATEGORIA)
			(PARTITION PRIMER_GRUPO VALUES LESS THAN ('F'),
			PARTITION SEGUNDO_GRUPO VALUES LESS THAN ('P'),
			PARTITION TERCER_GRUPO ALUESS LESS THAN ('Z')
			);
	
	
				CREATE TABLE  VARIOS
				  (COD NUMBER CONSTRAINT PK_VARIOS
				              PRIMARY KEY,
				   ANO NUMBER,
				   MES NUMBER,
				   DIA NUMBER
				   )
				   PARTITION BY RANGE(ANO,MES,DIA)
				    (PARTITION T11 VALUES LESS THAN(2022,4,1),
				     PARTITION T12 VALUES LESS THAN(2022,7,1),
				     PARTITION T13 VALUES LESS THAN(2022,10,1),
				     PARTITION T14 VALUES LESS THAN(2023,1,1),
				     PARTITION T21 VALUES LESS THAN(2023,4,1),
				     PARTITION T22 VALUES LESS THAN(2023,7,1),
				     PARTITION T23 VALUES LESS THAN(2023,10,1),
				     PARTITION T24 VALUES LESS THAN(2024,1,1)
				     )
				    TABLESPACE USERS;
### LISTA
	CREATE TABLE MATERIAS
			(CODIGO NUMBER PRIMARY KEY,
			NOMBRE VARCHAR2(30),
			PROFESOR VARCHAR2(30)
			)
			PARTITION BY LIST (PROFESOR)  
				(PARTITION SANTI VALUES ('SANTI','santi','s') TABLESPACE USUDAW
				PARTITION MLUZ VALUES('MLUZ','mluz','ml') TABLESPACE USUDAW2,
				PARTITION PABLO VALUES ('PABLO','pablo','p') TABLESPACE USUDAW,
				PARTITION PENDIENTE VALUES (DEFAULT) TABLESPACE USUDAW
				);
		Se necesita poner default, como max value de rango para las que no esten en lo listado
### Hash

### Intervalos
	SELECT NUMTOYMINTERVAL(1,'YEAR') FROM DUAL; SOLO YEARMONTH EN YM
		SELECT NUMTODSINTERVAL(1,'DAY') FROM DUAL; INTERVALOS DE DIA 
			DAY, HOUR,MINUTE,SECOND
	PARTICIONAMIENTO POR INTERVALOS
	
		FUNCIONA CON FECHA Y CON NUMEROS, CON VARCHAR2  ¡¡¡ NO !!

	
		CREATE TABLE NOSE1122
		   (NUM NUMBER,
		    NOM VARCHAR2(20),
		    FECHA DATE
		    )
		    PARTITION BY RANGE(FECHA)
		    INTERVAL(NUMTOYMINTERVAL(1,'YEAR'))
		    
		     ( PARTITION PANTE VALUES LESS THAN (TO_DATE('1-ENE-2022','DD-MON-YYYY'))
		     TABLESPACE USUDAW2
		     );
		
		este código crea una tabla particionada por rango basada en la columna "FECHA" con un intervalo de particionamiento de 1 año. La partición inicial incluye todas las fechas anteriores al 1 de enero de 2022 y se crearán automáticamente nuevas particiones cada año a partir de esa fecha.
		
					SELECT NUMTOYMINTERVAL(1,'YEAR') FROM DUAL;
					SELECT NUMTOYMINTERVAL(2,'YEAR') FROM DUAL;
					SELECT NUMTOYMINTERVAL(111,'MONTH') FROM DUAL;
					SELECT NUMTOYMINTERVAL(1,'DAY') FROM DUAL;
					
					SELECT NUMTODSINTERVAL(2,'DAY') FROM DUAL;
					SELECT NUMTODSINTERVAL(1,'HOUR') FROM DUAL;
					SELECT NUMTODSINTERVAL(1,'MINUTE') FROM DUAL;
					SELECT NUMTODSINTERVAL(1,'SECOND') FROM DUAL;

### Merge
	Los merges o fusiones de particiones son operaciones que permiten combinar dos o más particiones adyacentes en una sola partición más grande. 
	
	1. Merge en particionamiento por rango (RANGE):
	   - En el particionamiento por rango, las particiones se definen mediante un rango de valores especificado por la cláusula VALUES LESS THAN.
	   - Al realizar un merge en particiones por rango, se deben fusionar particiones adyacentes en orden ascendente.
	   - La nueva partición resultante tendrá el límite superior (VALUES LESS THAN) de la última partición fusionada.
	   - Ejemplo:

	     ALTER TABLE NOSE1122 MERGE PARTITIONS UNO, DOS INTO PARTITION DOS;
	     
	     En este caso, las particiones "UNO" y "DOS" se fusionarán en una sola partición llamada "DOS". La nueva partición "DOS" tendrá el límite superior de la partición original "DOS".
	
	2. Merge en particionamiento por lista (LIST):
	   - En el particionamiento por lista, las particiones se definen mediante una lista de valores específicos.
	   - Al realizar un merge en particiones por lista, se pueden fusionar particiones que contengan valores distintos.
	   - La nueva partición resultante contendrá la unión de los valores de las particiones fusionadas.
	   - Ejemplo:
	     ```sql
	     ALTER TABLE NOSE1122 MERGE PARTITIONS LISTA1, LISTA2 INTO PARTITION LISTA_COMBINADA;
	     ```
	     En este caso, las particiones "LISTA1" y "LISTA2" se fusionarán en una sola partición llamada "LISTA_COMBINADA". La nueva partición contendrá los valores de ambas particiones originales.
	
	3. Merge en particionamiento por función de hash (HASH):
	   - En el particionamiento por función de hash, las particiones se crean automáticamente según una función de hash aplicada a la columna de particionamiento.
	   - Al realizar un merge en particiones por función de hash, se pueden fusionar particiones contiguas.
	   - La nueva partición resultante tendrá un nuevo nombre y contendrá los datos de las particiones fusionadas.
	   - Ejemplo:
	     ```sql
	     ALTER TABLE NOSE1122 MERGE PARTITIONS HASH1, HASH2 INTO PARTITION HASH_COMBINADA;
	     ```
	     En este caso, las particiones "HASH1" y "HASH2" se fusionarán en una sola partición llamada "HASH_COMBINADA". La nueva partición contendrá los datos de ambas particiones originales.
	
	-- AL HECER UN ALTER TABLE MERGE PARTITION, EL INDICE SE QUEDA UNUSABLE, HAY QUE REINDEXAR.
					ALTER INDEX PK_MUTANTE REBUILD;


## E.4- Subparticiones

	ALTER TABLE ALUMNO2 ADD SUBPARTITION  NEURO VALUES('N','n');       --casca
			CASCCA PORQUE SI ESTA ESTABLECITA EN TEMPLATE NO PUEDES AÑADIR


			CREATE TABLE PERSONAS3
			  (DNI VARCHAR2(10),
			   NOM VARCHAR2(30),
			   GENERO CHAR(1),
			   CODIGO NUMBER,
			   CONSTRAINT PK_PERSONAS3
			     PRIMARY KEY(DNI)
			     )
			    TABLESPACE USUDAW
			    PARTITION BY RANGE(CODIGO)
			    SUBPARTITION BY RANGE(GENERO)
			      SUBPARTITION TEMPLATE
			      (SUBPARTITION HOMBRES VALUES LESS THAN ('I'),
			       SUBPARTITION MUJERES VALUES LESS THAN ('N')
			       )
			      (PARTITION UNO VALUES LESS THAN (2) TABLESPACE USERS,
			       PARTITION DOS VALUES LESS THAN(3) TABLESPACE USUDAW2,
			       PARTITION TRES VALUES LESS THAN (4) TABLESPACE USUDAW2 ,
			       PARTITION OTROS VALUES LESS THAN(MAXVALUE) TABLESPACE USERS
			       );


## E.5- STORAGE
			CREATE  TABLE  VARIOS3
			  (COD NUMBER CONSTRAINT PK_VARIOS3
			              PRIMARY KEY,
			   ANO NUMBER,
			   MES NUMBER,
			   DIA NUMBER,
			   GENERO CHAR(1),
			   CONSTRAINT C_CONTROLA
			      CHECK (ANO <=2030)  
			   )
			   PARTITION BY RANGE(ANO,MES,DIA)
			   SUBPARTITION BY LIST(GENERO)
			     SUBPARTITION TEMPLATE
			     (SUBPARTITION HOMBRES VALUES ('H','h'),
			      SUBPARTITION MUJERES VALUES ('M','m')
			      )  
			    (PARTITION T11 VALUES LESS THAN(2022,4,1),
			     PARTITION T12 VALUES LESS THAN(2022,7,1),
			     PARTITION T13 VALUES LESS THAN(2022,10,1),
			     PARTITION T14 VALUES LESS THAN(2023,1,1),
			     PARTITION T21 VALUES LESS THAN(2023,4,1),
			     PARTITION T22 VALUES LESS THAN(2023,7,1),
			     PARTITION T23 VALUES LESS THAN(2023,10,1),
			     PARTITION T24 VALUES LESS THAN(2024,1,1)
			     )
			   STORAGE 
			    ( INITIAL 128 K
			     NEXT 256 K
			     PCTINCREASE 50
			     MINEXTENTS 2
			     MAXEXTENTS 10
			     )
			    TABLESPACE USERS;
			    SELECT * FROM DUAL;
			
## E.6- SECUENCIAS 

	CREATE SEQUENCE NOMBRE 
	      [START WITH N]
	      [INCREMENT BY VALOR]
	      [MAXVALUE VALOR]
	      [MINVALUE VALOR]
	      [CYCLE/NOCYCLE]
	      [SIZE VALOR]
	      
		 CREATE SEQUENCE CONTADOR;  
		 
		 NOMAXVALUE -> NO HAY VALOR MÁXIMO
			NOCYCLE -> NO REPETIR NÚMERO
			MAXVALUE -> VALOR MÁXIMO
			CYCLE -> SE PUEDE REPETIR
			CACHE -> SE GUARDA EN CACHE
			
	      CREATE SEQUENCE SEC1 START WITH 1 INCREMENT BY 1 MAXVALUE 10 CYCLE CACHE 2;
		      
	  INSERT INTO NOSE1221 VALUES (NOMBRE_SEQUENCE.NEXVAL,1); -> INSERTAR VALOR 
	
	        DROP SEQUENCE NOMBRE_SEQUENCE;

## E.7- ÍNDICES

				CREATE [UNIQUE] INDEX NOMBRE
				      ON TABLA (A1 [ASC/DESC], [A2 ASC/DESC])
				  [TABLESPACE .....]
				  [STORAGE (........);
				
				DROP TABLE NOSES1122 PURGE;
				CREATE TABLE NOSES1122
				  (X NUMBER CONSTRAINT PK_NOSES1122
				            PRIMARY KEY,
				   XX NUMBER,
				   XXX NUMBER
				   );
				   
				SELECT * FROM USER_INDEXES WHERE TABLE_NAME='NOSES1122';  
				
				CREATE INDEX  INDICE_XX
				    ON NOSES1122(XX)
				    TABLESPACE USERS;
				CREATE UNIQUE INDEX  INDICE_XX
				    ON NOSES1122(XX)
				    TABLESPACE USERS;    
				    
				 INSERT INTO NOSES1122 VALUES(1,1,1);   
				 INSERT INTO NOSES1122 VALUES(2,1,1);      
				 
				DROP INDEX  INDICE_XX;   
				 
				CREATE INDEX  INDICE_XX_DD
				    ON NOSES1122(XX DESC)
				    TABLESPACE USERS;   
				    
				
				CREATE INDEX  INDICE_XXX
				    ON NOSES1122(XX,XXX DESC)
				    TABLESPACE USERS;


## E.8- COMPRESIÓN

	NOCOMPRESS PARA DECLARAR QUE NO SE COMPRIMA 
		SI DECLARAS FUERA UN TIPO DE COMPRESION, TODAS LAS PARTICIONES SE COMPRIMEN ASÍ, SALVO QUE 
		ESPECIFICES EN ELLAS UNA DIFERENTE, Y ENTONCES ESA TIENE PRIORIDAD.

			¡¡¡ NO TEMOPRALES, NO EXTERNAS!!!

	se puede hacer a nivel de tablespace, a nivel de particion, subparticion y a nivel de tabla
	 DOS SISTEMAS
	  BASICA   - MENOS - NO PODEMOS BORRAR COLUMNAS
	  AVANZADA - MAS NO HAY LIMITACION DE LAS COLUMNAS

	TABLESPACE
	PARTICION
	SUBPARTICION
	TABLA

	  CREATE OR REPLACE DIRECTORY RUTA AS 'C:\app\alumno\FICHEROS';
	DROP DIRECTORY FICHEROS;
	DROP TABLE BASICA;
	DROP TABLE AVANZADA;
	SELECT * FROM ALL_DIRECTORIES;
	GRANT READ,WRITE ON DIRECTORY RUTA TO ROL_DAW;
	
	SELECT * FROM V$PARAMETER
	   WHERE NAME LIKE '%block%';
	   
		● **AÑADE COMPRENSIÓN A TABLA CREADA**
		**ALTER TABLE** **NOMBRE** **COMPRESS FOR DIRECT_LOAD OPERATIONS; -> BÁSICA**
		**ALTER TABLE** **NOMBRE** **COMPRESS FOR ALL OPERATIONS; -> AVANZADA**
		
		● **ELIMINA COMPRESIÓN A UNA TABLA**
		**ALTER TABLE** **NOMBRE** **NO COMPRESS;**

		● **AÑADIR ATRIBUTOS (DEJA EN AMBAS):**
		**ALTER TABLE** **NOMBRE** **ADD (**campo2 TIPO**);**

		● **ELIMINAR ATRIBUTOS (SOLO DEJA EN OLTP)**
		**ALTER TABLE** **NOMBRE** **DROP (**campo2 **);**



	 CREATE TABLE AVANZADA2
	  (X NUMBER,
	   XX NUMBER,
	   Y CHAR(2000),
	   YY CHAR(2000)
	   ) 
	   PARTITION BY RANGE(X)
	    ( PARTITION INICAL VALUES LESS THAN(11) 
	        COMPRESS FOR DIRECT_LOAD OPERATIONS,
	      PARTITION DOS VALUES LESS THAN(21),
	      PARTITION TRES VALUES LESS THAN (31)
	        COMPRESS FOR ALL OPERATIONS ,
	      PARTITION CUATRO VALUES LESS THAN(41),
	      PARTITION CINCO VALUES LESS THAN(51) NOCOMPRESS            
	    )
	    COMPRESS FOR ALL OPERATIONS;    
## E.9- OPTIMIZAR 

	EXPLAIN PLAN FOR 
	 SELECT APELLIDO,LOC
	      FROM EMPLE22 E
	       INNER JOIN DEPART22 D
	        ON E.DEPT_NO=D.DEPT_NO;
	        
	SELECT * FROM TABLE(DBMS_XPLAN.display);            
	     
     
## E.10- ENCRIPTACIÓN

	SELECT * FROM V$ENCRYPTION_WALLET;
	ALTER SYSTEM SET KEY IDENTIFIED BY "jachis1000";
	ALTER SYSTEM SET WALLET OPEN IDENTIFIED BY "jachis1000";
	ALTER SYSTEM SET WALLET CLOSE;
	DROP TABLE OFUSCADA PURGE;
	
	CREATE TABLE OFUSCADA
	 (  X  NUMBER,
	    XX NUMBER,
	    Y NUMBER ENCRYPT USING 'AES256'  CONSTRAINT PK_OFUSCADA PRIMARY KEY,
	    YY NUMBER ENCRYPT
	    ); --CASCA POR PK ENCRIPTADA
	    
	CREATE TABLE OFUSCADA
	 (  X  NUMBER,
	    XX NUMBER,
	    Y NUMBER ENCRYPT USING 'AES256'  CONSTRAINT C_OFUSCADA CHECK (Y <= 100),
	    YY NUMBER ENCRYPT
	    ); --SOLO CHECK
	        
	ALTER TABLE OFUSCADA ADD (Z NUMBER ENCRYPT USING 'AES256');    
	    
	SELECT * FROM USER_ENCRYPTED_COLUMNS WHERE TABLE_NAME='OFUSCADA';    
	INSERT INTO OFUSCADA VALUES(1,1,1,1);
	     
	SELECT * FROM OFUSCADA;
	SELECT X,XX,Y FROM OFUSCADA;
	ALTER TABLE OFUSCADA MODIFY (X NUMBER ENCRYPT);
	
	GRANT ALTER SYSTEM TO SCOTT;
	    
	SELECT COLUMN_NAME FROM USER_TAB_COLUMNS
	     WHERE TABLE_NAME = 'OFUSCADA'
	     MINUS
	SELECT COLUMN_NAME  FROM USER_ENCRYPTED_COLUMNS
	     WHERE TABLE_NAME = 'OFUSCADA';
        
- `ALTER SYSTEM SET KEY IDENTIFIED BY "jachis1000";`
    - Esta sentencia establece una nueva contraseña maestra de cifrado para la cartera de claves. La contraseña especificada es "jachis1000".
- `ALTER SYSTEM SET WALLET OPEN IDENTIFIED BY "jachis1000";`
    - Esta sentencia abre la cartera de claves utilizando la contraseña maestra especificada en el paso anterior. Al abrir la cartera, Oracle puede acceder a las claves de cifrado almacenadas en ella.
- `ALTER SYSTEM SET WALLET CLOSE;`
    - Esta sentencia cierra la cartera de claves. Una vez cerrada, Oracle no puede acceder a las claves de cifrado almacenadas en la cartera hasta que se vuelva a abrir.

## E.11- CLUSTERS
	**- Crear cluster y después las tablas**
	
	**- No se puede añadir una tabla creada al cluster**
	
	**- No se puede indicar tablespace en tablas, lo decide el cluster**
	
	**- Las tablas del cluster no pueden estar particionadas**
	
	**- No podemos comprimir los clusters**
	
	**- No pueden ser tablas externas**
	
	**- No pueden tener atributos virtuales**

	DROP CLUSTER MIOCLUSTER;
	DROP CLUSTER MIOCLUSTER INCLUDING TABLES;
	DROP CLUSTER MIOCLUSTER INCLUDING TABLES CASCADE CONSTRAINTS;
	
	OPCIONES STORAGE 
	         TABLESPACE 
	         SIZE TAMAÑO
	         HASHKEYS MAXMINO NUM CLAVES PEVISTO
	         
	OPCIONALES     
	;    
	
	CREATE CLUSTER MIOCLUSTER (COL NUMBER(2))
	          TABLESPACE USUDAW
	          STORAGE (INITIAL 10 K
	                    NEXT 128 K
	                    PCTINCREASE 40
	                    )
	          SIZE 1024
	          HASHKEYS 100;
	          
	   
	
	CREATE TABLE EMPLEC
	      (NUM NUMBER PRIMARY KEY,
	       DEP NUMBER(2),    
	       SAL NUMBER,
	       APE VARCHAR2(20)
	       )
	       CLUSTER MIOCLUSTER(DEP);
	       
	CREATE TABLE DEPARTC
	      (DEP NUMBER(2) PRIMARY KEY,
	       LOC VARCHAR2(30)
	       )
	      CLUSTER MIOCLUSTER(DEP);
	      
	CREATE TABLE FUERACLUSTER
	  ( XX NUMBER REFERENCES DEPARTC
	  );
	        
	                 
	INSERT INTO EMPLEC SELECT EMP_NO,DEPT_NO,SALARIO,APELLIDO
	                   FROM EMPLE;   
	                   
	                                    
	               
	SELECT * FROM EMPLE ORDER BY EMP_NO;
	DROP TABLE EMPLE PURGE;
	SELECT * FROM EMPLE2;
	CREATE TABLE EMPLE AS SELECT * FROM EMPLE2;
	
	INSERT INTO DEPARTC SELECT DEPT_NO,LOC FROM DEPART;
	
	SELECT * FROM DEPART;
	
	SELECT * FROM EMPLEC;
	SELECT * FROM DEPARTC;
	
	SELECT APE,LOC
	   FROM EMPLEC E
	    INNER JOIN DEPARTC D
	      ON E.DEP=D.DEP;
	-----------------------------
	ERRORES  SI TIENE HASHKEY NO TABLESPACE
	-----------------------------
	;
	CREATE CLUSTER MIOCLUSTER (COL NUMBER(2))
	          TABLESPACE USUDAW
	          STORAGE (INITIAL 10 K
	                    NEXT 128 K
	                    PCTINCREASE 40
	                    )
	          SIZE 1024
	          HASHKEYS 100;
	          
	   
	
	CREATE TABLE EMPLEC
	      (NUM NUMBER PRIMARY KEY,
	       DEP NUMBER(2),    
	       SAL NUMBER,
	       APE VARCHAR2(20)
	       )
	       CLUSTER MIOCLUSTER(DEP)
	       TABLESPACE USUDAW;   --CASCA
	       
	 CREATE TABLE EMPLEC
	      (NUM NUMBER PRIMARY KEY,
	       DEP NUMBER(2),    
	       SAL NUMBER,
	       APE VARCHAR2(20)
	       )
	       CLUSTER MIOCLUSTER(DEP)
	       TABLESPACE USUDAW;   --CASCA
	       
	       
	       
	 CREATE CLUSTER MIOCLUSTER (COL NUMBER(2))
	          TABLESPACE USUDAW
	          STORAGE (INITIAL 10 K
	                    NEXT 128 K
	                    PCTINCREASE 40
	                    )
	          SIZE 1024;
	          
	 CREATE TABLE EMPLEC
	      (NUM NUMBER PRIMARY KEY,
	       DEP NUMBER(2),    
	       SAL NUMBER,
	       APE VARCHAR2(20)
	       )
	       CLUSTER MIOCLUSTER(DEP);
	       
	CREATE TABLE DEPARTC
	      (DEP NUMBER(2) PRIMARY KEY,
	       LOC VARCHAR2(30)
	       )
	      CLUSTER MIOCLUSTER(DEP);     
	      
	SELECT * FROM EMPLEC;   
	SELECT * FROM DEPARTC;   
	
	CREATE INDEX INDICECLUSTER ON CLUSTER  MIOCLUSTER;
	
	SELECT * FROM USER_TABLES WHERE TABLE_NAME IN ('EMPLEC','DEPARTC');
	
	
	SELECT * FROM ASIGNATURAS;
	SELECT * FROM NOTAS;
	SELECT * FROM ALUMNOS;
	
	CREATE CLUSTER OTROCLUSTER(COD VARCHAR2(10))
	     SIZE 1024
	     TABLESPACE USERS
	     HASHKEYS 100
	     ;
	     DROP TABLE ALUMNOSC PURGE;
	     DROP TABLE NOTASC PURGE;
	     
	CREATE TABLE ALUMNOSC
	   (DNI VARCHAR2(10) PRIMARY KEY,
	    NOM VARCHAR2(20),
	    EDAD NUMBER,
	    CODIGO NUMBER
	    )
	    CLUSTER OTROCLUSTER(DNI);
	    
	CREATE TABLE NOTASC
	  (DNI VARCHAR2(10),
	   COD NUMBER(2),
	   NOTA NUMBER(2)
	   ) CLUSTER OTROCLUSTER(DNI);
	   
	CREATE TABLE ASIGNATURASC AS SELECT * FROM ASIGNATURAS;  
	ALTER TABLE ASIGNATURASC ADD CONSTRAINT PK_ASIGNATURASC PRIMARY KEY(COD);
	
	INSERT INTO ALUMNOSC SELECT * FROM ALUMNOS;
	INSERT INTO NOTASC SELECT * FROM NOTAS;
	
	SELECT * FROM USER_TABLES WHERE TABLE_NAME IN ('ALUMNOSC','NOTASC','ASIGNATURASC');
	ALTER TABLE NOTASC ADD CONSTRAINT FK_NOTAS_ASIGNATURAS FOREIGN KEY(COD) REFERENCES ASIGNATURASC(COD);

Sinonimos


## E.12- COMENTARIOS 

	**COMMENT ON TABLE** **NOMBRE** **IS 'NO SE QUE PONER'; -> CREAR COMENTARIO**
	
	**COMMENT ON TABLE** **NOMBRE** **IS ''; -> BORRAR COMENTARIO**
	
	**CREATE TABLE** **NOMBRE**
	
	**(**campo1 TIPO **COMMENT** **'Nombre del cliente',**
	
	campo2 TIPO
	**);


## E.13- Vistas

### Vistas NO Materializadas


	

### Vistas Materializadas


# F- 4_EJERCICIOSs
## F.1- *CERO DIAGRAMA*
	CREATE TABLE AUTOR
	(CODIGO_A NUMBER CONSTRAINT PK_AUTOR PRIMARY KEY,
	NOMBRE VARCHAR2(20)
	);
	
	CREATE TABLA LIBRO 
	(CODIGO_L NUMBER CONSTRAINT PK_LIBRO PRIMARY KEY,
	TITULO VARCHAR2(30),
	ISBN NUMBER (10),
	EDITORIAL VARCHAR2(10),
	PAGINAS NUMBER
	);
	
	CREATE TABLE ESCRIBEN_LIBROS
	(CODIGO_A CONSTRAINT FK_ESC_AUTOR REFERENCES AUTOR(CODIGO_A),
	CODIGO_L CONSTRAINT FK_ESC_LIBRO REFERENCES LIBRO(CODIGO_L),
	CONSTRAINT PK_ESCRIBEN_LIBROS PRIMARY KEY(CODIGO_A,CODIGO_L)
	);
	
	CREATE TABLE EJEMPLAR 
	(CODIGO_E NUMBER,
	LOCALIZACION VARCHAR2(20),
	CODIGO_L CONSTRAINT FK_EJEMPLAR_LIBRO REFERENCES LIBRO(CODIGO_L)
	);
	
	CREATE TABLE USUARIO
	(CODIGO_U NUMBER CONSTRAINT PK_USUARIO PRIMARY KEY,
	NOMBRE VARCHAR2(20),
	TELEFONO NUMBER(9),
	DIRECCION VARCHAR2(20)
	);
	
	CREATE TABLE PRESTAMOS_EJEMPLARES
	(CODIGO_U NUMBER CONSTRAINT FK_PRES_USUARIO REFERENCES USUARIO(CODIGO_U),
	CODIGO_E NUMBER CONSTRAINT FK_PRES_EJEMPLAR REFERENCES EJEMPLAR(CODIGO_E),
	CONSTRAINT PK_PRES_EJEMPLARES PRIMARY KEY(CODIGO_U,CODIGO_E)
	);
## F.2- *ESQUEMA_HOSPITAL*
	
	CREATE TABLE HOSPITAL1
	(HOSPITAL_COD CONSTRAINT PK_HOSPITAL1 PRIMARY KEY 
	NOMBRE VARCHAR2(20) NOT NULL,
	DIRECCION VARCHAR2(20),
	TELEFONO NUMBER(9),
	NUM_CAMAS NUMBER CONSTRAINT C_NUM_CAMAS 
					CHECK (NUM_CAMAS BETWEEN 100 AND 5000)
	);
	CREATETABLE DOCTOR1
	(DOCTOR_NO CONSTRAINT PK_DOCTOR1 PRIMARY KEY 
	APELLIDO VARCHAR2(20) NOT NULL,
	ESPECIALIDAD VARCHAR2(20),
	SALARIO NUMBER DEFAULT 500000,
	HOSPITAL_COD CONSTRAINT FK_DOCTOR1_HOSPITAL1 REFERENCES HOSPITAL1(HOSPITAL_COD),
	CONSTRAINT C_DOCTOR_NO 
		CHECK (DOCTOR_NO NOT IN(100,10)
		AND DOCTOR_NO > 5)
	);
	CREATE TABLE SALA1
	(SALA_COD CONSTRAINT PK_SALA1 PRIMARY KEY 
	NOMBRE VARCHAR2(20) NOT NULL
	NUM_CAMAS NUMBER,
	HOSPITAL_COD CONSTRAINT FK_SALA1_HOSPITAL1 REFERENCES HOSPITAL1(HOSPITAL_COD),
	CONSTRAINT C_NOMBRE 
		CHECK (NOMBRE LIKE ('PISO%')),
	CONSTRAINT C_NUM_CAMAS 
		CHECK (NUM_CAMAS BETWEEN 50 AND 100)
	);
	CREATE TABLE PLANTILLA1
	(EMPLEADO_NO CONSTRAINT PK_PLANTILLA1 PRIMARY KEY 
	APELLIDO VARCHAR2(20) NOT NULL,
	FUNCION VARCHAR2(20) NOT NULL,
	TURNO VARCHAR2(20) NOT NULL,
	SALARIO NUMBER,
	SALA_COD CONSTRAINT FK_PLANTILLA1_SALA1 REFERENCES SALA1(SALA_COD),
	CONSTRAINT C_TURNO
		CHECK (TURNO IN('M','T','N')),
	CONSTRAINT C_FUNCION
		CHECK (FUNCION IN('ENFERMERO','ENFERMERA','INTERNO'))
	);
	CREATE TABLE OCUPACION1
	(CAMA NUMBER CONSTRAINT PK_OCUPACION PRIMARY KEY,
	SALA_COD CONSTRAINT FK_OCUPACION1_SALA1 REFERENCES SALA1(SALA_COD)
	);
	CREATE TABLE ENFERMO1
	(INSCRIPCION CONSTRAINT PK_ENFERMO1 PRIMARY KEY
	APELLIDO VARCHAR2(20),
	DIRECCION VARCHAR2(20),
	FECHA_NAC DATE,
	SEXO VARCHAR2(10),
	NSS NUMBER
	);

## F.3- *ENUNCIADO7*

## F.4- *ENUNCIADO8*

## F.5- *ENUNCIADOSMIOS*
### UNO
	Crea una tabla con los campos dni, nombre, fecha de nacimiento y edad.
	Edad es un atributo virtual. Tendrá que valer entre 0 y 120 años.
	La tabla estará particionada por intervalos de edad de 20 años cada uno.

	CREATE TABLE EJUNO
	(DNI VARCHAR2(9),
	NOMBRE VARCHAR2(20),
	F_NAC DATE,
	EDAD GENERATED ALWAYS AS ( ) VIRTUAL
	)
	PARTITION BY RANGE (EDAD) INTERVAL 20
	(PARTITION E1 VALUES LESS THAN(20)
	);
### DOS
	¿ Y si quitamos el atributo virtual edad y su constraint y particionamos
	por fecha_nacimiento ?
	Con intervalos de nacidos antes de 1960, 1980, 2000, 2020 y el resto

	ALTER TABLE EJ1 DROP COLUMN EDAD;

### TRES
	Consulta en la tabla anterior los nacidos antes de 1960 y posteriores al
	año 2000.

	SELECT NOMBRE, DNI 
	FROM EJ1
	WHERE (EXTRACT(YEAR FROM F_NAC)) < 
	1960 OR (EXTRACT(YEAR FROM F_NAC)) > 2000;
### CUATRO
	Añade a la tabla anterior un campo curso que sólo puede valer PRIMERO,
	SEGUNDO O TERCERO


### CINCO
	Crea una tabla con los campos dni, nombre, fecha de nacimiento.
	Crea un campo curso con los posibles varlores PRIMERO, SEGUNDO O TERCERO,
	y subparticiona la tabla por ese campo.
	La tabla estará particionada por intervalos de matricula de 300 erusos
	cada uno.
	La tabla se creará en el tablespace users
### SEIS
	¿Qué he de hacer para copiar toda la particion que se creó inicialmente
	de la tabla
	anterior en otra tabla?


### SIETE
	¿Cómo puedo juntar las particiones segunda y tercera creadas en la
	tercera?


### OCHO
	¿Cómo puedo mejorar las búsquedas por el campo fecha_nacimiento de la
	tabla anterior?


# G- Apuntes sucio
## G.1- *18-03-24*
	BLOQUE ANONIMO: 
		DECLARE (ES OPCIONAL)
		hoy DATE; 
		// o sin select hoy:= SYSDATE;
		BEGIN
		SELECT SYSDATE 
			INTO hoy
			FROM DUAL;
		DBMS_OUTPUT.PUT_LINE('Hoy es: ' || TO_CHAR (hoy, 'day,dd', "de" " month " del " yyyy'));
		END;
	ejemplo: 
		<<fuera>>
	declare 
	 numero pls_integer;
	begin
	 numero:=100;
	 <<dentro>>
	   DECLARE 
	   numero pls_integer;
	   numero_dentro pls_integer;
	   BEGIN
	     numero:=200;
	     DBMS_OUTPUT.PUT_LINE('VALOR DE NUMERO : ' ||numero);
	     DBMS_OUTPUT.PUT_LINE('VALOR DE NUMERO_DENTRO : ' ||numero_denrto);
	    END DENTRO;
	         DBMS_OUTPUT.PUT_LINE('VALOR DE NUMERO : ' ||numero);
	     DBMS_OUTPUT.PUT_LINE('VALOR DE NUMERO_DENTRO : ' ||numero_denrto); -- CASCA
	end fuera;
	
	Para acceder desde dentro a la variable de fuera que se llama igual, especificamos con el ambito y la variable : fuera.numero
	
	Con documento tabla: nos pida un codigo, y decimos "el codigo es " + producto
		DECLARE 
		producto varchar2(30);
		BEGIN 
		SELECT TARTICULOS.DENOM 
			INTO producto
			FROM TARTICULOS;       ampersand y variable externa
			WHERE TARTICULOS.CODIGO = &DATO; -- "pide por consola"
			DBMS_OUTPUT.PUT_LINE('EL CODIGO: ' || &DATO || ' SON ' || producto);
		END;
		
	pidiendo el nombre de producto y nos de el codigo
	DECLARE 
		codigo tarticulos.codigo%type;
		BEGIN 
		SELECT TARTICULOS.CODIGO 
			INTO codigo
			FROM TARTICULOS;      
			WHERE TARTICULOS.DENOM = &PRODUCTO; -- NECESITA APÓSTROFE  o con '&PRODUCTO' SIEMPRE CON APOSTROFE
			DBMS_OUTPUT.PUT_LINE('EL PRODUCTO: ' || &producto || ' tiene codigo ' || codigo);
		END;
		
	para producto es mejor poner :
		producto tarticulos.denom%type; para que sse ajuste al tipo de la columna
		
		
	metiendo 10 tuplas en una tabla:
	Creamos una tabla:
		CREATE TABLE CARGAR (
		N NUMBER,
		NN VARCHAR2(30)
		);
		
		DECLARE 
		I PLS_INTEGER;
		BEGIN
			DBMS_OUTPUT.PUT_LINE('VALOR DE '|| I) -- si i no tiene valor da null
			LOOP
				INSERT INTO CARGAR VALUES(I,'TEXTO : ' || I);
				I:=I+1;
				IF I>10 THEN 
				EXIT;  -- EXIT WHEN MEJOR, EN CLASE 1 ESTÁ
				END IF;
			END LOOP;
			COMMIT WORK;-- SI UN BLOQUE NO COMMITA, SE BORRA AUTOMATICAMENTE, COMMIT PARA GUARDAR
		END;
## G.2- *21-03-24*
DECLARE 
CUANTOS PLS_INTEGER;

BEGIN 
  SELECT MAX(TARTICULOS.CODIGO)+1
  INTO CUANTOS
  FROM TARTICULOS;
  INSERT INTO TARTICULOS(CODIGO,DENOM) VALUES (CUANTOS,'&datos');
  COMMIT WORK; -- Importante para que se guarden los cambios
  DBMS_OUTPUT.put_line('INSERTADO EL PRODUCTO ' || '&datos' || ' CON EL CODIGO: ' || cuantos);

END;

  CREATE TABLE DINERO 
  (CODIGO NUMBER, IMPORTE NUMBER);
  INSERT INTO DINERO VALUES (1,500);
    INSERT INTO DINERO VALUES (2,1000);
      INSERT INTO DINERO VALUES (3,1500);
        INSERT INTO DINERO(CODIGO) VALUES (4);

	CREATE TABLE RPTA(CODIGO NUMBER,CALIFICATION VARCHAR(80));

	DECLARE 
	 V_IMPORTE DINERO.IMPORTE%TYPE;
	 TEXTO VARCHAR2(20);
	BEGIN 
	 SELECT IMPORTE 
	       INTO V_IMPORTE
	       FROM DINERO
	       WHERE CODIGO = &dato;
	       IF V_IMPORTE IS NULL THEN
	          TEXTO:='NO CONTEMPLADO';
	       ELSIF V_IMPORTE <= 500 THEN
	         TEXTO:='GANAS POCO';
	       ELSIF V_IMPORTE <= 1000 THEN
	         TEXTO:='GANAS NORMAL';
	       ELSIF V_IMPORTE <= 1500 THEN
	         TEXTO:='GANAS BASTANATE';
	       ELSE 
	         TEXTO:='GANAS COMO UN POLITICO';
	       END IF;
	       DBMS_OUTPUT.PUT_LINE('EL CODIGO ' || &dato || TEXTO);
	       INSERT INTO RPTA VALUES (&dato,TEXTO);
	       COMMIT WORK;
       
	DECLARE 
	CONTADOR PLS_INTEGER;
	BEGIN
	  SELECT COUNT(*) 
	  INTO CONTADOR
	  FROM RPTA WHERE CALIFICACION ='NO CONTEMPLADO';
	  
	  UPDATE  RPTA SET CALIFICACION = NULL WHERE  CALIFICACION ='NO CONTEMPLADO';
	  COMMIT WORK;
	  DBMS_OUTPUT.PUT_LINE('SE HAN BORRADO : ' || CONTADOR || ' TUPLAS');
	END;
	   END;
	   
	Con sentencia CASE:
	WHEN... THEN
	END CASE
	
	BUCLES FOR:
	
	  CREATE TABLE PRUEBA (X NUMBER, CC VARCHAR2(30));
  
	  BEGIN
	    FOR I IN 1..10 LOOP -- I IN REVERSE 1..10 EMPIEZA POR 10
	      INSERT INTO PRUEBA2 VALUES(I,'TEXTO'||I);
	    END LOOP;
	    COMMIT WORK;
	  END;
	
	-- ejercicio de dar la vuelta al apellido
	
	PROCEDIMIENTOS ALMACENADOS:
	
		CREATE OR REPLACE PROCEDURE INVERTIR (P_NUM IN NUMBER)   -- NO SE PUEDEN RESTRINGIR EN TAMAÑO , VARCHAR2 A SECAS AQUI
	AS 
	LETRA VARCHAR2(80);
	V_APELLIDO EMPLE.APELLIDO%TYPE;
	BEGIN
	  SELECT APELLIDO 
	  INTO V_APELLIDO
	  FROM EMPLE
	  WHERE EMP_NO = P_NUM;
	  
	  FOR I IN REVERSE 1..length(V_APELLIDO) LOOP
	    LETRA := LETRA || SUBSTR(V_APELLIDO,I,1);
	  END LOOP;
	   DBMS_OUTPUT.put_line(V_APELLIDO|| ' AL REVES ' || LETRA ) ; 
	END INVERTIR;
	
	
	A VERLO EN EL DICCIONARIO DE DATOS:
	
	SELECT * FROM USER_SOURCE; 
	CODIGO DE ESE USUARIO
	
	 SELECT * FROM USER_OBJECTS;
	 WHERE OBJECT_TYPE = 'PROCEDURE';
	 
	 DERECHOS DE EXECUTE 
	 GRANT EXECUTE ON INVERTIR TO ROL_DAW;
	 
	 a la izquierda en carpetas-> procedures
	 -> ubicarlo, -> test, crea un bloque anonimo para testearlo(debugger)
	 
	 Otra manera de llamarlo:
	 
	 CREATE OR REPLACE PROCEDURE LLAMOINVERTIR(P_NUM IN NUMBER)
	 AS 
	 BEGIN
		 INVERTIR(P_NUM);
		 
	 END LLAMOINVERTIR;
	
	
## G.3- *04-04-24*
		 -- un procedimiento al que se le pase 2 parametro, el emp_no y el nuevo oficio
		 CREATE OR REPLACE PROCEDURE P_CAMBIAR_OFICIO (P_EMP_NO IN EMPLE.EMP_NO%TYPE, P_NUEVO_OFICIO IN EMPLE.OFICIO%TYPE)
		 AS 
		 ACTUAL EMPLE.OFICIO%TYPE;
		 
		 BEGIN
		   SELECT OFICIO FROM EMPLE 
		   INTO ACTUAL
		   FROM EMLPE
		   WHERE EMP_NO = P_EMP_NO;
		   
		   UPDATE EMPLE 
		      SET OFICIO = P_NUEVO_OFICIO;
		      WHERE EMP_NO = P_NUM;
		      
		   COMMIT WORK; 
		   DBMS_OUTPUT.put_line('ANTERIOR OFICIO: ' || ACTUAL)
		      DBMS_OUTPUT.put_line('NUEVO OFICIO: ' || P_NUEVO_OFICIO)
		
		 END P_CAMBIAR_OFICIO;			
		 
	BEGIN 
		P_CAMBIAR_OFICIO(P_EMP_NO => 7369, P_NUEVO_OFICIO => 'ABOGADO');
	END;				
	
	-- OTRO procedimiento, haciendolo con apellido y nuevo oficio, no hace falta hacer otro bloque cambiando el parametro de emp_no por apellido. Se puede reaprovechar el procedimiento de cambiar. 
	
		CREATE OR REPLACE PROCEDURE LLAMOCAMBIAR (P_APELLIDO IN EMPLE.APELLIDO%TYPE,
											P_NUEVO_OFICIO IN VARCHAR2
		AS
		V_NUM EMPLE.EMP_NO%TYPE;
		BEGIN
			SELECT EMP_NO INTO V_NUM
				FROM EMPLE
				WHERE APELLIDO= P_APELLIDO;
			PROFE.CAMBIAR (P_NUM => V_NUM, P_OFI => P_NUEVO_OFICIO);  
		END LLAMOCAMBIAR;
	
		BEGIN
			PROFE.LLAMOCAMBIAR('SÁNCHEZ','NOSE');
		END;
		
		-- AHORA LLAMANDO DESDE UN PROCEDIMIENTO ALMACENADO
		
		CREATE OR REPLACE PROCEDURE LLAMOCAMBIARNOMBRE (P_NOMB IN VARCHAR2,
												P_OFI IN VARCHAR2)
			AS
			BEGIN
				PROFE.LLAMOCAMBIAR(P_NOM => P_NOM, P_NUEVO=> P_OFI);
			END LLAMOCAMBIARNOMBRE;
			
		-- SE PUEDE DEBUGGEAR TODO LO ANTERIOR LINEA A LINEA Y VA SALTANDO A LOS PROCE.
		-- RECORDATORIO:  LAS SELECT DEVUELVAN SOLO UNA TUPLA! IMPORTANTE (POR AHORA)!!
		
	-otro ejercicio: procedimiento, le pasamoss 1 numero de departamento y nos devuelve el total numero de empleados que tiene, la media salarial y suma salarial de ese dept
	
	CREATE OR REPLACE PROCEDURE P_CALCULARDPT (P_DEPT_NO IN EMPLE.DEPT_NO%TYPE, P_TOTAL OUT NUMBER, P_MEDIA OUT NUMBER, P_SUMA OUT NUMBER)
	AS
	BEGIN
		SELECT COUNT(*) INTO P_TOTAL   -- SE PUEDE HACER EN UN SELECT ME CAGO EN TODO
			FROM EMPLE
				WHERE P_DEPT_NO=DEPT_NO
				GROUP BY DEPT_NO
		SELECT AVG(SALARIO) INTO P_MEDIA
			FROM EMPLE
			WHERE P_DEPT_NO=DEPT_NO
			GROUP BY DEPT_NO;
		SELECT SUM(SALARIO) INTO P_TOTAL
			FROM EMPLE
			WHERE P_DEPT_NO=DEPT_NO
			GROUP BY DEPT_NO;
	END P_CALCULARDPT;
	
	-- otro procedimiento que le pasamos dept_no, y llama al anterior para visualizad datos, luego debug de este 
	
	CREATE OR REPLACE PROCEDURE LLAMAR_CALCULARDPT(P_NUM_DPT NUMBER)
	AS
	v_total pls_integer;
	v_media number;
	v_suma number;
	BEGIN
		P.CALCULARDPT(P_DEPT_NO => P_NUM_DPT, MEDIA=>V_MEDIA,TOTAL => V_TOTAL,SUMA=>V_SUMA);
		DBMS_OUTPUT.PUT_LINE('blabla');
	END;
	
## G.4- *05/04/24*
	-- le pasamos un numero de empleado y le sacamos el jefe
	CREATE OR REPLACE PROCEDURE NOMBREJEFE(P_EMP_NO IN NUMBER)
		AS
		V_NOMBREJEFE EMPLE.APELLIDO%TYPE;
		BEGIN
			SELECT APELLIDO 
				INTO v_nombreJefe
				FROM EMPLE
				WHERE EMP_NO = (SELECT DIR  
								FROM EMPLE
								WHERE EMP_NO= P_EMP_NO);
			DBMS_OUTPUT.PUT_LINE('EL JEFE DE : ' || P_EMP_NO || ' ES ' || V_NOMBREJEFE)
		END NOMBREJEFE;
	
	BEGIN
    
    |FUNCIONES ! una que pasamos nombre de empleado y nos devuelve el numero de emp
    sintaxis: (PARA TESTEAR, ESTA EN OBJETOS FUNCTION)
    
		CREATE OR REPLACE FUNCTION EMPLEADO (P_NOM IN VARCHAR2)
			RETURN EMPLE.EMP_NO%TYPE (O NUMBER, PLS_INTEGER...)--RETURN OBLIGATORIO, QUE TIPO DE DATOS RETORNA 
			AS
			V_NUM EMPLE.EMP_NO%TYPE;
			BEGIN
				SELECT EMP_NO 
					INTO V_NUM
					FROM EMPLE
					WHERE APELLIDO = P_NOM;
			RETURN V_NUM;
			END;
	- LAS FUNCIONES SE PUEDEN INTEGRAR EN SQL 
	  SELECT APELLIDO, SALARIO 
		  FROM EMPLE
		  WHERE EMP_NO = EMPLEADO('GIL');
		  
	SELECT EMPLEADO('GIL')
		FROM DUAL; 
		
	- UN BLOQUE QUE LLAME A LA FUNCION 
		CREATE OR REPLACE PROCEDURE LLAMOEMPLEADO(P_NOM IN VARCHAR2)
		AS
		V_NUM NUMBER(4);
		BEGIN
			V_NUM:= EMPLEADO(P_NOM);
			DBMS_OUTPUT.PUT_LINE(V_NUM);
		END LLAMOEMPLEADO;
		
	- otro procedimiento se pasa nombre nuevo oficio. tenemos uno hecho con oficio nuevo y numero y lo cambia, y tenemos la funcion que pasando nombre, nos retorna numero, la reutilizamos para reutilizar a su vez el de cambiar. 
	  
		CREATE OR REPLACE PROCEDURE OTROCAMBIAR (P_NOM VARCHAR2, P_OFI VARCHAR2)
		AS 
			NUM NUMBER,
		BEGIN
			NUM:=EMPLEADO(P_NOM);
			CAMBIAR(NUM,P_OFI);
			DBMS_OUTPUT.PUT_LINE('HECHO');
		END OTROCAMBIAR;
	
	- FUNCION QUE LE PASAMOS EL NUMERO DE UN DEPARTAMENTO Y NOS DA NOMBRE
		  CREATE OR REPLACE FUNCTION NOMBREDEPART (P_DEPT_NO DEPART.DEPT_NO%TYPE) // MIA
		  RETURN  DEPART.NOMBRE%TYPE
		  BEGIN 
			  SELECT NOMBRE INTO V_NOMBRE_DEPART
				  FROM EMPLE
				  WHERE DEPT_NO = P_DEPT_NO;
		  RETURN V_NOMBRE
		  END NOMBREDEPART;
	- FUNCION QUE LE PASAMOS NUMERO Y NOS RETORNA LA MEDIA DE SALARIO
		  CREATE OR REPLACE FUNCTION MEDIASALARIO (P_DEPT_NO DEPART.DEPT_NO%TYPE)//MIA
			  RETURN NUMBER;
			  BEGIN 
				  SELECT AVG(SALARIO) INTO v_media
					  FROM EMPLE
					  WHERE DEPT_NO = P_DEPT_NO;
					  GROUP BY DEPT_NO;
			  RETURN v_media;
			  END MEDIASALARIO;
	- FUNCION LE PASAMOS EL NOMBRE DE UN DEPARTAMENTO Y NOS RETORNA LA SUMA SALARIAL
	  
	  CREATE OR REPLACE FUNCTION SUMATOTALDEPT (P_NOM_DEPT DEPART.NOMBRE%TYPE)
		  RETURN NUMBER
		  BEGIN 
			  SELECT SUM(SALARIO) INTO v_suma_salarial
				  FROM EMPLE
				  WHERE DEPT_NO = (SELEC DEPT_NO
								  FROM DEPART 
								  WHERE NOMBRE = P_NOM_DEPT)
				  GROUP BY DEPT_NO;
		  RETURN V_NOMBRE
		  END SUMA;
		  
	SELECT DEPARTAMENTO(10),DEPARTAMENTO(20)
	FROM DUAL;
	SELECT SUMATOTALDEPT(NOMBREDEPART(20))
		FROM DUAL;
		
	| FUNCIONES DETERMINISTICAS - QUE ANTE LOS MISMOS PARAMETROS DE ENTRADA , DEVUELVE LOS MISMOS RESULTADOS. 
		- NO MIRA EN EL DICCIONARIO DE DATOS
	¡¡UNA FUNCION EN UN ATRIBUTO VIRTUAL TIENE QUE SER MARCADA COMO DETERMINISTICA!!
	 -- no emplear en un check
	 
	 - FUNCION QUE CALCULA LA LETRA DE DNI
	
		CREATE OR REPLACE FUNCTION LETRANIF(P_DNI IN PLS_INTEGER)
		RETURN CHAR
		DETERMINISTIC -- IMPORTANTE PARA EMPLEARLA COMO ATRIBUTO VIRTUAL
		AS 
		RPTA CHAR(1);
		RESTO PLS_INTEGER;
		BEGIN
			RESTO:=MOD(P_DNI,23);
			CASE RESTO
			WHEN 0 THEN rpta:='T'; 
			WHEN 1 THEN rpta:='R'; 
			WHEN 2 THEN rpta:='W'; 
			WHEN 3 THEN rpta:='A'; 
			WHEN 4 THEN rpta:='G'; 
			WHEN 5 THEN rpta:='M'; 
			WHEN 6 THEN rpta:='Y'; 
			WHEN 7 THEN rpta:='F'; 
			WHEN 8 THEN rpta:='P'; 
			WHEN 9 THEN rpta:='D'; 
			WHEN 10 THEN rpta:='X'; 
			WHEN 11 THEN rpta:='B'; 
			WHEN 12 THEN rpta:='N'; 
			WHEN 13 THEN rpta:='J'; 
			WHEN 14 THEN rpta:='Z'; 
			WHEN 15 THEN rpta:='S'; 
			WHEN 16 THEN rpta:='Q'; 
			WHEN 17 THEN rpta:='V'; 
			WHEN 18 THEN rpta:='H'; 
			WHEN 19 THEN rpta:='L'; 
			WHEN 20 THEN rpta:='C'; 
			WHEN 21 THEN rpta:='K'; 
			WHEN 22 THEN rpta:='E';
			END CASE;
			RETURN RPTA;
		END;
		
	CREATE TABLE SUJETO1
	(NON VARCHAR2(30),
	DNI NUMBER,
	NIF GENERATED ALWAYS AS (DNI ||'-'|| LETRANIF(DNI))
	);
	
## G.5- *08-04-24*
	CREATE OR REPLACE FUNCTION EDAD(F_NAC IN DATE)
		RETURN PLS_INTEGER
		DETERMINISTIC -- PARA PODER EMPLEAR EN LA EXPRESION DE UN ATRIB. VIRTUAL
		AS
		  ANIOS PLS_INTEGER;
			BEGIN
				ANIOS:= TRUNC(MONTHS_BETWEEN(SYSDATE,F_NAC)/12);
			RETURN ANIOS;
		END EDAD;
	
		-Ahora a hacer un atrib virtual en una tabla 
	CREATE TABLE POLITICOS
	(NOM VARCHAR2(30),
	FECHA DATE, 
	EDAD_C GENERATED ALWAYS AS -- EDAD CASCA PORQUE NO SE PUEDE LLAMAR = QUE LA FUNCION
		(EDAD(FECHA)) VIRTUAL
	);
	
	- funcion booleana, que retorne boolean, le pasamos el nombre de un emple, nos retorna true si su salario > que el departamento donde trabajo, falso  si es menos, null si gana lo mismo. 
	  
	CREATE OR REPLACE FUNCTION ALEATORIA (NOMBRE IN PROFE.EMPLE.APELLIDO%TYPE)
	RETURN BOOLEAN 
	AS 
		v_salario PROFE.EMPLE.SALARIO%TYPE;
		V_MEDIA PLS_INTEGER;
		SW BOOLEAN;
	BEGIN 
		SELECT SALARIO 
			INTO V_SSALARIO
			FROM EMPLE
			WHERE APELLIDO=NOMBRE;
		SELECT ROUND(AVG(SALARIO))
			INTO V_MEDIA
			FROM EMPLE
			WHERE DEPT_NO = (SELECT DEPT_NO 
							FROM EMPLE 
							WHERE APELLIDO=NOMBRE);
		CASE 
			WHEN V_SALARIO < V_MEDIA THEN
				SW:=FALSE;
			WHEN V_SALARIO > V_MEDIA THEN 
				SW:=TRUE;
			ELSE SW:= NULL;
		END CASE;
		RETURN SW;
	END ALEATORIA;
	
	- Un procedimiento que llame a la de antes, y que visualice el resultado
		CREATE OR REPLACE PROCEDURE LLAMAR_ALEATORIA (NOMBRE IN EMPLE.APELLIDO%TYPE)
		AS
		SW BOOLEAN;
		TEXTO VARCHAR2(200);
		BEGIN
			SW:= ALEATORIA(NOMBRE);
			CASE SW
				WHEN TRUE THEN TEXTO:='GANAS MAS QUE LA MEDIA DE TU DPT';
				WHEN FLASE THEN TEXTO:='GANAS MENOS QUE LA MEDIA DE TU DPT';
				WHEN NULL THEN TEXTO:='GANAS IGUAL QUE LA MEDIA DE TU DPT';
			END CASE;
			DBMS_OUTPUT.PUT_LINE(TEXTO);
		END LLAMAR_ALEATORIA;
		
## G.6- *11-04-24*  c6
	al igual que las variables globales, los parametros de la rutina de jerarquia mayor, son globales y se pueden usar en las subrutinas de dentro. 
	
	una subrutina puede llamar a otra (calcular media que llame a calculardepartamento). Siempre que no esté "delante" que no lo dejaría compilar. Se puede solucionar con prototipado, declarando la funcion antes solo con la cabecera para que 'conozca' la funcion.
	
	Otra manera: partir los bloques del ej. ASIGNAR en objetos. 
	las funciones/proce -> CREATE OR REPLACE FUNCTION/PROCE...  (ALMACENADO), el codigo de por si de asignar del begin, pasado a procedure.

	- excepciones y limitacion de consultas. 
	  
	  BEGIN 
	    PROFE.ASIGNAR5('ALONSO');
		  EXCEPTION
		  WHEN E1 THEN
		  WHEN E2 THEN  
	   END;
	 -- le paso el nombre de emple y nos dice el dept (empleado2)
	no hace falta try/catch
	bloque: EXCEPTION
		(GESTORES 1.PREDEFINIDOS2.USUARIO.3.PRAGMAS)
		WHEN NO_DATA FOUND THEN...
		WHEN (V_DEPT NUMBER(1) NO VA A CABER 20 EN 1) VALUE_ERROR THEN DBMS_OUTPU....
		WHEN TOO_MANY_ROWS..
	si comentara too_many_rows o no me supiera una excepcion pero quisiera capturarla, se usa con el comodín WHEN OTHERS , tiene que ser la ultima porque si no no entra en las anteriores mas especificas (y no compila) 
	CODIGO ERROR: SQLCODE
	TEXTO ERROR: SQLERRM
	
	un procedimiento alm.: le pasamos una fecha , sacamos del empleado que se dio de alta en esa fecha el apellido, el oficio el nombre del departamento y la localidad. Y vamos a gestionar errores (que no se diera de alta 1, o que haya varios, y por si acaso un when others). Grabamos esos errores en una tabla (la fecha, SYSDATE, usuario, SQLCODE, SQLERRM).
	
	CREATE OR REPLACE PROCEDURE FECHAEMPLE (FECHA IN DATE) 
	AS 
	V_APELLIDO EMPLE.OFICIO%TYPE;
	V_OFICIO EMPLE.OFICIO%TYPE;
	V_DPT EMPLE.DEPT_NO%TYPE;
	V_LOC DEPART.LOC%TYPE;
	BEGIN
		SELECT APELLIDO 
			INTO V_APELLIDO
			FROM EMPLE
			WHERE FECHA_ALT=FECHA;
			
		SELECT OFICIO
		INTO V_OFICIO
			FROM EMPLE
			WHERE FECHA_ALT=FECHA;
		SELECT DEPT_NO
		INTO V_DPT
			FROM EMPLE
			WHERE FECHA_ALT=FECHA;
			
		SELECT LOC 
		INTO V_LOC
		FROM DEPART
		WHERE DEPT_NO = (SELECT DEPT_NO 
		FROM EMPLE 
		WHERE FECHA_ALT=FECHA);
-- SE PUEDE HACER EN 1 ME CAGO EN TODO X2 	

	CREATE PROCEDURE LOGERROR

		EXCEPTION
		WHEN TOO_MANY_ROWS THEN 
		DBMS_OUTPUT.PUT_LINE('HAY VARIOS EMPLEADOS DADOS DE ALTA EN ESA FECHA');
		WHEN NO_DATA_FOUND THEN 
		DBMS_OUTPUT.PUT_LINE('NO HAY NINGUN EMPLE DADO DE ALTA EN ESA FECHA');
	
		
	END FECHAEMPLE;
## G.7- *15-04-24*
	EXCEPCIONES DE USUARIO, HAY QUE DECLARARLAS, ES DE UN TIPO DE DATO : EXCEPTION 
	EN EL PROCEDIMIENTO CONSULTA DE CLASE 
		E_JEFE EXCEPTION;
		E_VENTAS EXCEPTION; iniciarlas con e_nombre
		
	A devolver le añadimos: V_APELLIDO Y V_DNOMBRE
		 PROCEDURE DEVOLVER(V_APELLIDO IN VARCHAR2, V_DNOMBRE IN VARCHAR2,V_USU OUT VARCHAR2,V_FECHA OUT DATE,
	                    V_CODIGO OUT PLS_INTEGER,V_TEXTO OUT VARCHAR2)
	   AS  
	   BEGIN
	     SELECT USER INTO V_USU FROM DUAL;
	     	     V_FECHA:=SYSDATE;
	     IF V_APELLIDO='SÁNCHEZ' THEN
		     V_CODIGO:=200;
		     V_TEXTO:='SE HAN VISTO DATOS DE SÁNCHEZ');
		    ELSIF V_DNOMBRE = 'VENTAS' THEN 
		     V_CODIGO:=201;
		     V_TEXTO:='SE HAN VISTO DATOS DE GENTE DE VENTAS');
		    ELSE 

	     V_CODIGO:=SQLCODE;
	     V_TEXTO:=SQLERRM;
	     END IF; 
	   END DEVOLVER; 
	
	Y en guardar cambiamos fecha: 
	
		 PROCEDURE GUARDAR (FECHA IN DATE, V_CODIGO IN PLS_INTEGER,V_TEXTO IN VARCHAR2,
	                    V_FECHA IN DATE, V_USU IN VARCHAR2
	                    )
	   AS
	   BEGIN
	     INSERT INTO INCIDENCIAS VALUES (V_FECHA,FECHA,V_USU,V_CODIGO,V_TEXTO);
	     COMMIT WORK;
	     DBMS_OUTPUT.put_line('REGISSTRADA INCIDENCIA');
	   END GUARDAR;   
	
	Y en exception cambiamos:
	
	 EXCEPTION
	   WHEN NO_DATA_FOUND THEN
	      DEVOLVER (V_APELLIDO,V_DNOMBRE,V_USU,V_FECHA,V_CODIGO,V_TEXTO);
	      GUARDAR(FECHA,V_CODIGO,V_TEXTO,V_FECHA,V_USU);
	   WHEN VALUE_ERROR THEN
	     DEVOLVER (V_APELLIDO,V_DNOMBRE,V_USU,V_FECHA,V_CODIGO,V_TEXTO);
	      GUARDAR(FECHA,V_CODIGO,V_TEXTO,V_FECHA,V_USU);
	   WHEN TOO_MANY_ROWS THEN
	      DEVOLVER (V_APELLIDO,V_DNOMBRE,V_USU,V_FECHA,V_CODIGO,V_TEXTO);
	      GUARDAR(FECHA,V_CODIGO,V_TEXTO,V_FECHA,V_USU);   
	   WHEN E_JEFE THEN
	      DEVOLVER (V_APELLIDO,V_DNOMBRE,V_USU,V_FECHA,V_CODIGO,V_TEXTO);
	      GUARDAR(FECHA,V_CODIGO,V_TEXTO,V_FECHA,V_USU);   
	   WHEN E_VENTAS THEN
	      DEVOLVER (V_APELLIDO,V_DNOMBRE,V_USU,V_FECHA,V_CODIGO,V_TEXTO);
	      GUARDAR(FECHA,V_CODIGO,V_TEXTO,V_FECHA,V_USU);   
	   WHEN OTHERS THEN
	      DEVOLVER (V_APELLIDO,V_DNOMBRE,V_USU,V_FECHA,V_CODIGO,V_TEXTO);
	      GUARDAR(FECHA,V_CODIGO,V_TEXTO,V_FECHA,V_USU);
	En el bloque main del begin:
	
	BEGIN
	  SELECT APELLIDO,OFICIO,LOC, DNOMBRE
	      INTO V_APELLIDO,V_OFICIO,V_LOC,V_DNOMBRE
	      FROM EMPLE E
	       INNER JOIN DEPART D
	       ON E.DEPT_NO=D.DEPT_NO
	      WHERE FECHA_ALT = FECHA;    
	      
	      IF V_APELLIDO ='SÁNCHEZ' THEN 
		      RAISE E_JEFE
		    ELSEIF V_DNOMBRE = 'VENTAS' THEN
			    RAISE E_VENTAS;
		   END IF;
	  DBMS_OUTPUT.put_line('APELLIDO: ' || V_APELLIDO);
	  DBMS_OUTPUT.put_line('OFICIO: ' || V_OFICIO);
	  DBMS_OUTPUT.put_line('LOCALIDAD: ' || V_LOC);
	  DBMS_OUTPUT.put_line('NOMBRE: ' || V_DNOMBRE);
	  
	- PRAGMAS , entre predefinidas y las de usuario. 
	  
	  CREATE TABLE ACCESO NUM NUMBER NOT NULL, NOMBRE VARCHAR2(30),A DATE AA DATE);
	  INSERT INTO ACCESO(NOMBRE) VALUES ('PEPE'); ERROR -1400 NO VALOR CAMPO OBLIGATORIO
		  VAMOS A CONVERTIR EN PREDEFINIDA ESTE ERORR QUE NO TIENE EXCEPT. PREDEFINIDA 
		  
	  CONSULTA 3 - CAMBIOS
	  SE DECLARAN LOS PRAGMAS EN EL PRIMER AS, 
		  1. CREAMOS UNA EXCEPCION DE USUARIO 
				  E_CAMPO_OBLIGATORIO EXCEPTION; 
				  -- 2 PARAMETROS PARA PRAGMA  (EXCPECION DE USUARIO)
				  PRAGMA EXCEPTION_INIT(E_CAMPO_OBLIGATORIO,-1400);
				  NO HACE FALTA HACER RAISE!! PORQUE ES UN PRAGMA (PREDEF.->USU)
		AÑADIMOS AL CAMPO DE EXCEPTIONLA DE E_CAMPO OBLIGATORIO
		
		WHEN E_CAMPO_OBLIGATORIO THEN
	      DEVOLVER (V_APELLIDO,V_DNOMBRE,V_USU,V_FECHA,V_CODIGO,V_TEXTO);
	      GUARDAR(FECHA,V_CODIGO,V_TEXTO,V_FECHA,V_USU);   
	      
	      EN EL AREA DEL MAIN BEGIN HACEMOS INSERCIONES PARA PROBARLA: 
		      INSERT INTO ACCESO(NOMBRE,A,AA) VALUES (V_APELLIDO,FECHA,SYSDATE);
		      COMMIT WORK; 
	      
	 - RAISE APLICATION ERROR  :  -20.000  Y -20.999
	   
	   EN EL BLOQUE PRINCIPAL DE BEGIN 
	   
	   IF V_OFICIO = 'ANALISTA' THEN  -- LO CAPTURA EL WHEN OTHERS
		   RAISE_APPLICATION_ERROR(-20000,'NO SE PUEDE ES ANALISTA');
	  END IF;
	  
		  SIN UN WHEN OTHERS SALE VENTANA DEL ERROR. 
		  
		  -- PROCEPADRE1 Y  PROCEHIJO1 EN CLASE VERLO
		  -- INCIDENCIA LA GUARDA EL HIJO, EL FLUJO DE EJECUCION SIGUE Y SE INSERTA EN ACCESO
		  -- QUITANDO NO DATA FOUND Y WHEN OTHERS PARA QUE PROPAGUE, SE PONEN EN PADRE
		  EN PADRE NO RECONOCE DEVOLVER Y GUARDAR, QUIERE LLAMARLAS PERO NO SON SUS SUBRUTINAS, HAY QUE PONERLOS COMO PROCEDIMIENTOS ALMACENADOS 
		  
		  -- OTRO CASO SERIA QUE EL HIJO NO TRATE LA EXCEPCION (NODATAFOUND O WHEN OTHERS, NO INSERTA EN ACCESO, RESPONDE EL PADRE Y VA A LA ZONA DE EXCEPCIONES)
		  
		  select * from emple for update que hace?
		  
		  begin 
			
			procepadre2(fecha => fecha);
		end;
## G.8- *18-04-24* 
	ejemplo de procehijo sin bloque aparte y todo en uno
	
 - CURSORES : +de una tupla
	 se declaran despues del AS, CON IS 
		 3 TIPOS:
			 1.ESTATICOS: UNA VARIABLE TIPO CURSOR-> UNA SELECT				 2. VARIABLE TIPO CURSOR: 
		      3. CURSORES DINAMICOS
	 
		 CREATE OR REPLACE PROCEDURE
		 CURSOR1 (DEP IN EMPLE.DEPT_NO%TYPE)
		 AS 
		 V_ACOPLA EMPLE.DEPT_NO%TYPE;
		 
		 CURSOR C1 IS SELECT APELLIDO,DNOMBRE,LOC
			 FROM EMPLE E 
				 INNER JOIN DEPART D 
				  ON E.DEPT_NO=D.DEPT_NO
				  WHERE E.DEPT_NO = V_ACOPLA ;-- QUITAR AMBIGÜEDAD CON E 
			 V_APE EMPLE.APELLIDO%TYPE;
			 V_NOM DPEART.DNOMBRE%TYPE;
			 V_LOC DEPART.LOC%TYPE;
			BEGIN
				V_ACOMPLA:=DEP;
				OPEN C1;
					FETCH C1 INTO V_APE, V_NOM,V_LOC; -- PARA VER SI HAY DATOS/DA ERROR
					
				  WHILE C1%FOUND LOOP
			DBMS_OUTPUT.PUT_LINE(TUPLE: ' || C1%ROWCOUNT);	  DBMS_OUTPUT.PUT_LINE(V_APE|| ' - ' ||V_NOM||' - '||V_LOC);
			FETCH C1 INTO V_APE, V_NOM,V_LOC;
				  
				  
				  END LOOP;
		  - o con LOOP END LOOP; Y UN EXIT WHEN C1%NOTFOUND DESPUES DEL FETCH
				CLOSE C2; 
			END CURSOR1;
		- LOS CURSORES HAY QUE 1. ABRIRLOS (EJECUTA SELECT) ANTES DE ABRIRLO TODAS LAS VARIABLES DE ACOPLAMIENTO TIENEN QUE TENER VALORES CIERTOS (V_ACOPLA) . LUEGO BUCLE WHILE. TODOS LOS CURSORES TIENEN 4 ATRIBUTOS. 1C1%FOUND-2C1%NOTFOUND-3C1%ROWCOUNT-4C1%ISOPEN . LOS DINAMICOS NO LAS TIENEN
		  2. ACCEDER DATOS
		     
		CURSOR 4 -> SIN VARIABLESS PARA VOLCAR, USA REG C1%ROWTYPE; (1 VARIABLE EN VEZ DE 3, DESPUES DEL CURSOR). FETCH C1  INTO REG . PARA SACARLOS, REG.APELLIDO POR EJEMPLO.

		SI NO ENCUENTRA DATOS PODEMOS PONER UNA EXCEPCIONCON UN IF DE C1%ROWCOUNT=0 THEN RAISE...
		NO SALTA NO DATA FOUND EN LA SELECT DE UN CURSOR, POR ESO EXCEPCION DE USUARIO 
	
	
		OTRA MANERA DE HACER LOS CURSORES  - CON UN FOR (ANTES WHILE Y LOOP)
			FOR REG IN C1 LOOP  (NO HACE FALTA DECLARAR REG)
			
			END LOOP; -- NO HACE FALTA OPEN, CLOSE Y FETCH, NI DECLARAR VARIABLE DONDE VOLCAR.
		EN CURSOR7, LA EXCEPCION ES INVALID CURSOR, PORQUE AL NO ABRIR EL CURSOR, FUERA DEL LOOP C1%NOTFOUND NO SE PUEDE ACCEDER PARA LA EXCEPCION NO_DATOS, PORQUE EL CURSOR NO SE HA ABIERTO.
			MIRAR EL SW DE CURSOR7?.
			
			SIN DECLARAR EL CURSOR EN CURSOR8 PARA AHORRAR MEMORIA. SIN ATRIBUTOS.

## G.9- *19-04-24*
CREATE TABLE ALUMNOS ( DNI VARCHAR2(10) NOT NULL, APENOM VARCHAR2(30), DIREC VARCHAR2(30), POBLA VARCHAR2(15), TELEF VARCHAR2(10) ); INSERT INTO ALUMNOS VALUES ('12344345','Alcalde García, Elena', 'C/Las Matas, 24','Madrid','917766545'); INSERT INTO ALUMNOS VALUES ('4448242','Cerrato Vela, Luis', 'C/Mina 28 - 3A', 'Madrid','916566545'); INSERT INTO ALUMNOS VALUES ('56882942','Díaz Fernández, María', 'C/Luis Vives 25', 'Móstoles','915577545');

## G.10- *22-04-24*
	 Cursores sincronizados: 
	 
	 vamos a poner el nombre del departamento y luego vamos a hacer una relacion donde vamos a poner los nombres de los trabajadores y los que ganan

	CREATE OR REPLACE PROCEDURE 
	       SINCRO1
	 AS
	 ACOPLA EMPLE.DEPT_NO%TYPE;
	  CURSOR CDEPART IS      
	         SELECT DNOMBRE,DEPT_NO
	                FROM DEPART;
	  CURSOR CEMPLE IS
	         SELECT APELLIDO,SALARIO+ NVL(COMISION,0) AS TOTAL
	         FROM EMPLE
	         WHERE DEPT_NO = ACOPLA; -- HAY QUE DECLARAR PREVIAMENTE, VARIABLE DE ACOPLAMIENTO
	         
	  REGE CEMPLE%ROWTYPE;
	 
	 BEGIN
	   FOR REGD IN CDEPART LOOP
	     DBMS_OUTPUT.PUT_LINE('DEPT: ' || REGD.DNOMBRE);
	     ACOPLA:= REGD.DEPT_NO;
	     OPEN CEMPLE;
	     FETCH CEMPLE INTO REGE;
	  
	     WHILE CEMPLE%FOUND LOOP
	           DBMS_OUTPUT.PUT_LINE(REGE.APELLIDO || ' === ' || REGE.TOTAL);
	           FETCH CEMPLE INTO REGE;
	     
	     
	     END LOOP;
	     CLOSE CEMPLE; -- IMPORTANTE
	 
	   END LOOP;
	 END;
	-- nombe del alumno, y los nombres de la asignatura y su nota


	  CURSORES QUE ACTUALIZAN/BLOQUEANETS
	- PROCEDIMIENTO CON 2 PARAMETROS, UN DEPARTAMENTO Y UNA SUBIDA DE SALARIO

		WHERE CURRENT OF > WHERE ROWID > WHERE....
	
	
	-- A TODOS LOS DE UN DEPARTAMENTO LE VAMOS A PONER LA MEDIA DE SU DEPARTAMENTO, TODOS LOS DEPARTAMENTOS


## G.11- 25-04-24 FALTA

## G.12- *26-04-24*
	- VAMOS A HACER QUE EN UNA SOLA VARIABLE CURSOR VALGAN VARIAS SELECT

	TYPE MICUR IS REF CURSOR;  -> A PASARLO COMO PARAMETRO 
	C1 MICUR; -- DECLARANDO LA VARIABLE
	
	CAMBIA LA SINTAXIS... OPEN C FOR... SELECT  
	LAS VARIABLES TIPO CURSOR NO TIENEN %ROWTYPE, NO FUNCIONA!
	
		CREATE TABLE OPERACIONES_PROCESO ( TIPO CHAR(1), PAR1 VARCHAR2(40), PAR2 NUMBER(9)); CREATE TABLE OPERACIONES_BACKUP ( TIPO CHAR(1), PAR1 VARCHAR2(40), PAR2 NUMBER(9)); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1) VALUES('A','MADRID'); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1,PAR2) VALUES('C','BARCELONA',2); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1,PAR2) VALUES('C','MADRID',3); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1) VALUES('A','BARCELONA'); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1,PAR2) VALUES('D','ANALISTA',4); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1,PAR2) VALUES('D','PRESIDENTE',20); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1,PAR2) VALUES('D','EMPLEADO',9); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1) VALUES('B','VENDEDOR'); INSERT INTO OPERACIONES_PROCESO(TIPO,PAR1) VALUES('B','ANALISTA'); COMMIT WORK;

	hacer ejercicio en casa: lo de procesos y backup, borrar y pasar a backup cuando se procese. 
		PARA DDL: EXECUTE IMMEDIATE 'TRUNCATE TABLE OPERACIONES_PROCESO';
	PROCEVARIOS4 SOLUCION
	
	- MIRAR LO DE RECORD 
	
	
	- ARRAYS DE UN TIPO VAMOS A VER: INDEXADOS 
	  
		  CREATE OR REPLACE PROCEDURE VECTOR1
	AS
	
	CURSOR CDATOS IS SELECT E.DEPT_NO, DNOMBRE, ROUND(AVG(SALARIO)) AS MEDIA
	       FROM EMPLE E 
	            INNER JOIN DEPART D 
	            ON D.DEPT_NO = E.DEPT_NO
	            GROUP BY E.DEPT_NO, DNOMBRE;
	            
	-- DECLARAMOS 3 VECTORES
	
	TYPE TDEP IS TABLE OF EMPLE.DEPT_NO%TYPE
	                      INDEX BY BINARY_INTEGER;
	TYPE TNOM IS TABLE OF DEPART.DNOMBRE%TYPE
	                      INDEX BY BINARY_INTEGER;
	TYPE TMED IS TABLE OF NUMBER
	                      INDEX BY BINARY_INTEGER;
	                      
	                      
	VDEP TDEP;
	VNOM TNOM;
	VMED TMED;
	     I PLS_INTEGER DEFAULT 0;
	
	BEGIN 
	     FOR REG IN CDATOS LOOP
	       I:=I+1;
	       VDEP(I):= REG.DEPT_NO;
	       VNOM(I):=REG.DNOMBRE;
	       VMED(I):= REG.MEDIA;
	       DBMS_OUTPUT.PUT_LINE(VDEP(I)||'---'||VNOM(I)||'---'||VMED(I));
	       
	     END LOOP;

	END VECTOR1;                  

## G.13- *29-04-24*  ¡FALTA!

	CLASE 13 Y TRIGGERS !!! 
## G.14- *06-05-24*
		CREATE OR REPLACE TRIGGER NOINSERTAR
	   BEFORE INSERT 
	   ON VEREMPLE
	   
	 BEGIN
	   IF USER<>'PROFE'THEN
	         RAISE_APPLICATION_ERROR(-20000,USER || ' NO PUEDES INSERTAR PERSE A LOS GRANT');
	   END IF;
	   DBMS_OUTPUT.put_line('insertado');
	 END NOINSERTAR;      

	Aun con grant 
	- no se pone FOR EACH ESTATEMENT, LOS QUE NO SEAN XEACH ROW SON ESO, SI SE PONE DA ERROR, ¡comprobar esto!
	
	CREATE OR REPLACE TRIGGER NOINSERTAR
		   BEFORE INSERT 
		   ON VEREMPLE
		   FOR EACH STATEMENT (ERROR! MAL!)
	
	- PARA DESHABILITAR TODOS LOS DE UNA TABLA TENEMOS:
		ALTER TABLE EMPLE DISABLE ALL TRIGGERS; 
		ALTER TRIGGER NOINSERTAR DISABLE; 
	
	disparador que solo deje trabajar con la tabla(domingos y sabados no,) viernes de 9 a 3. el resto de 9 a 6

	CREATE OR REPLACE TRIGGER HORAS
	  BEFORE INSERT OR DELETE OR UPDATE
	  ON EMPLE -- ES FOR EACH STATEMENT, NO NOS INTERESA LA INFORMACION, SI NO EL CONTROLAR EL MANEJO DE LA TABLA CON ESTE TRIGGER
	  DECLARE 
	  DIA VARCHAR2(40);
	  HORA PLS_INTEGER;
	  NO EXCEPTION;
	  BEGIN
	  SELECT TO_CHAR(SYSDATE,'DAY'),TO_NUMBER(TO_CHAR(SYSDATE,'HH24'),99)
	         INTO DIA,HORA
	         FROM DUAL;
	  CASE 
	    WHEN DIA LIKE 'DOMINGO%' OR DIA LIKE 'SÁBADO%' THEN
	         RAISE NO;
	    WHEN DIA LIKE 'VIERNES%' AND (HORA > 18 OR 
	                                HORA < 9) THEN
	         RAISE NO;
	         
	    ELSE IF HORA>18 OR HORA<9 THEN
	      RAISE NO;
	    END IF;
	  END CASE;
	      DBMS_OUTPUT.PUT_LINE('ACTUALIZADO');
	
	    DBMS_OUTPUT.PUT_LINE(DIA || '---'|| HORA);
		COMMIT WORK; -- NO SE PUEDE HACER COMMIT EN UN DISPARADOR! CASCA! 
	  EXCEPTION 
	    WHEN NO THEN 
	      RAISE_APPLICATION_ERROR(-20000,'EL DIA '|| DIA || ' NO SE PUEDE ' || HORA);
	 
	 - los disparadores no pueden commitar, se tiene que commitar desde fuera. Se puede hacer que funcione haciendo que forme parte de una transaccion distinta, con un pragma. 
	- si ponemos PRAGMA AUTONOMOUS_TRANSACTION en la parte de DECLARE. LO VAMOS A VER MEJOR DESPUES CON TRIGGERS CON COMMITS Y OPERACIONES DML
	  
	  - PARA USAR EL DEBUGGER PARA TRIGGERS, METEMOS LA OPERACION DML EN UN PROCEDIMIENTO : 
				  CREATE OR REPLACE PROCEDURE TESTEO 
					AS
					UPDATE EMPLE
					SET SALARIO = SALARIO+1
					WHERE DEPT_NO=20;
				END TESTEO;
	 
	-- REGISTRANDO CUALQUIER CAMBIO SOBRE EL SALARIO EN EMPLE (ANTES Y DESPUES NO SE PUEDEN OBTENER EN UN FOR EACH STATEMENT, SE TIENE QUE USAR EL FOR EACH ROW)
		CREATE TABLE REGISTRO
		(USUARIO VARCHAR2(40),
		FECHA DATE,
		ANTE NUMBER
		NUEVO NUMBER,
		EMPLEADO VARCHAR2(40));
		
		CREATE OR REPLACE TRIGGER CONTROLASALARIO
		AFTER UPDATE OF SALARIO -- SOLO POR CAMBIOS DE SALARIO
		ON EMPLE
		FOR EACH ROW -- NECESARIO PARA REGISTRAR LOS CAMBIOS
		-- EN FOR EACH ROW SE DECLARAN :OLD Y :NEW (PSEUDOREGISTROS), MISMA ESTRUCTURA (ROWTYPE QUE LA TABLA DE DISPARO, EN ESTE CASO EMPLE)
		DECLARE
			PRAGMA AUTONOMOUS_TRANSACTION;
		BEGIN
			INSERT INTO REGISTRO VALUES(USER,SYSDATE,:OLD.SALARIO,:NEW.SALARIO,:OLD.APELLIDO);
			DBMS_OUTPUT.PUT_LINE('REGISTRADO EL CAMBIO');
		
		-- COMMIT WORK CASCARIA , AHORA CON EL PRAGMA NO, COMMITA REGISTRO NO EMPLE
		- LO MALO ES QUE SI SE HACE ROLLBACK DE EMPLE SE DESINCRONIZA
		
		END CONTROLASALARIO;
	
		- EJ SOBRE NOTAS/ALUMNOS 
		- AL BORRAR UN ALUMNO QUE SE BORREN TODAS LAS BOTAS DE ESE ALUMNO EN LA TABLA DE NOTAS
			CREATE OR REPLACE TRIGGER BORRARALUMNO
		AFTER DELETE 
		ON ALUMNOS
		FOR EACH ROW
		  
		DECLARE
		BEGIN
		  DELETE FROM NOTAS 
		         WHERE DNI = :OLD.DNI;
		  DBMS_OUTPUT.put_line('BORRADAS ' || SQL%ROWCOUNT || ' NOTAS DE ' || :OLD.APENOM);
		END BORRARALUMNO;
		
		
		-- EN DEPART SI CAMBIAMOS EL NUMERO DE UN DEPARTAMENTO, AUTOMATICAMENTE NOS LO CAMBIE EN EMPLE 
			CREATE OR REPLACE TRIGGER ACTUALIZARDEPT 
	  AFTER UPDATE OF DEPT_NO  ON DEPART
	  FOR EACH ROW
	  
	  DECLARE
	  BEGIN
	    UPDATE EMPLE
	           SET EMPLE.DEPT_NO = :NEW.DEPT_NO
	           WHERE EMPLE.DEPT_NO = :OLD.DEPT_NO;
	  
	  END ACTUALIZARDEPT;
		
		!!EN LOS DISPARADORES BEFORE FOR EACH ROW, PODEMOS CAMBIAR LOS VALORES DE :NEW
			CREATE TABLE PRUEBA3 (NUM NUMBER NOT NULL,NOM VARCHAR2(20));
			INSERT INTO PRUEBA3 VALUES('PEPE'); -- CASCA, VAMOS A HACER QUE FUNCIONE
		
		BEFORE INSERT ON PRUEBA3 FOR EACH ROW....
		
				:NEW.NUM:= 1; LO SOBREESCRIBE, TANTO SI LO METE EL USER O NO .
			(MIRAR LO DE SEQUENCE PARA METERLE NUMERITOS)
		
		
## G.15- *09-05-24*
	orden cuando hay varios triggers (orden de ejecución)
	
	CRETE TABLE PRUEBA200
	(X NUMBER,
	XX VARCHAR2(40
	);
	CREATE SEQUENCE 
	
	
	CREATE OR REPLACE TRIGGER 
       ANTESORDEN 
       BEFORE UPDATE
       ON EMPLE
       
       BEGIN
          INSSERT INTO PRUEBA200
                  VALUES(SEC.NEXTVAL,'BEFORE ORDEN');
       END ANTESORDEN;
       
    CREATE OR REPLACE TRIGGER 
       DESPUESORDEN 
       AFTER UPDATE
       ON EMPLE
       
       BEGIN
          INSSERT INTO PRUEBA200
                  VALUES(SEC.NEXTVAL,'AFTER ORDEN');
       END ANTESORDEN;
       
       
       CREATE OR REPLACE TRIGGER 
       ANTEFILA 
       BEFORE UPDATE
      ON EMPLE
	  FOR EACH ROW 

       
       BEGIN
          INSSERT INTO PRUEBA200
                  VALUES(SEC.NEXTVAL,'ANTEFILA ');
       END ANTEFILA;
       
       CREATE OR REPLACE TRIGGER 
       DESPUESFILA 
       AFTER UPDATE
      ON EMPLE
	  FOR EACH ROW 

       BEGIN
          INSSERT INTO PRUEBA200
                  VALUES(SEC.NEXTVAL,'DESPUESFILA ');
       END DESPUESFILA;
	    
	1º DE TODOS EL BEFORE STATEMENT 
		- ENTRE MEDIAS FOR EACH ROW PARA CADA TUPLA 
	ÚLTIMO EL AFTER DE STATEMENT 
	
	-Ahora probando cuando son del mismo tipo, en qué orden se ejecutan:
	CREATE OR REPLACE TRIGGER 
		NOCONTROL1
		BEFORE DELETE 
		ON EMPLE
		FOR EACH ROW
			BEGIN
			DBMS_OUTPUT.PUT_LINE('SE EJECUTA NOCONTROL1');
		END NOCONTROL1;
		
		CREATE OR REPLACE TRIGGER 
		NOCONTROL2
		BEFORE DELETE 
		ON EMPLE
		FOR EACH ROW
			BEGIN
			DBMS_OUTPUT.PUT_LINE('SE EJECUTA NOCONTROL2');
		END NOCONTROL2;
		
	 PARA TENER CERTEZA DEL ORDEN, USAMOS LA CLÁUSULA FOLLOWS:
		CREATE OR REPLACE TRIGGER 
		SICONTROL2
		BEFORE DELETE 
		ON EMPLE
			FOR EACH ROW
			FOLLOWS SICONTROL1
			BEGIN 
				DBMS_OUTPUT.PUT_LINE('SE EJECUTRA SINCONTROL2');
		END SICONTROL2;
	
		CREATE OR REPLACE TRIGGER 
		SICONTROL3
		BEFORE DELETE 
		ON EMPLE
			FOR EACH ROW
			FOLLOWS SICONTROL2
			BEGIN 
				DBMS_OUTPUT.PUT_LINE('SE EJECUTRA SINCONTROL3');
		END SICONTROL3;
		
		el orden seria 1 -> 2 -> 3
	
	
	CREATE TABLE TIPOS 
		(USU VARCHAR2(30),
		FECHA DATE,
		TIPO CHAR(1),
		APELLIDO VARCHAR2(30)
		);
	
	Con esta tabla hacemos un disparador para saber que operacion estamos haciendo
		predicado: UPDATING,INSERTING, DELETING (solo en los triggers multievento)
		
		CREATE OR REPLACE TRIGGER 
			AUDITAR
			AFTER INSERT OR UPDATE OR DELETE
			ON EMPLE
			FOR EACH ROW
				DECLARE 
					QUE CCHAR(1);
					DATO VARCHAR2(40);
				BEGIN
					CASE
						WHEN INSERTING THEN 
							QUE:='I';
							DATO:= :NEW.APELLIDO;
						WHEN DELETING THEN
							QUE:='D';
							DATO:= :OLD.APELLIDO;
						WHEN UPDATING THEN 
							QUE:='U';
							DATO:= :NEW.APELLIDO;
					END CASE;
				INSERT INTO TIPOS VALUES (USER,SYSDATE,QUE,DATO);
				DBMS_OUTPUT.PUT_LINE('REGISTRADO');
				END AUDITAR;
				
				
		 - La clausula WHEN en los disparadores for each row, filtra para saber si se ejecuta en esa tupla concreta 
		   
		   SELECT * FROM EMPLE WHERE SALARIO > 220000; SALEN DE VARIOS DEP_NO

		 QUEREMOS UN DISPARADOR SOLO PARA LOS QUE SEAN DEL 10 Y DEL 20 
		 
		 UPDATE EMPLE
		 SET SALARIO = 2 
		 WHERE SALARIO > 220000;
		 
		 CREATE OR REPLACE TRIGGER 
			AUDITAR
			AFTER INSERT OR UPDATE OR DELETE
			ON EMPLE
			FOR EACH ROW   -- EN WHEN NO HACE FALTA PONER :NEW U :OLD ':'
				WHEN (NEW.DEPT_NO IN (10,20) OR OLD.DEPT_NO IN(10,20) )
		 ;
	
	
		- NO SE ADMITEN SUBIDAS SALARIALES POR ENCIMA DEL 10%
		  CREATE OR REPLACE TRIGGER 
			SALARIOS
			BEFORE UPDATE 
			ON EMPLE
			FOR EACH ROW   
				WHEN (NEW.SALARIO > OLD.SALARIO*1.10)
			  BEGIN
				  RAISE_APPLICATION_ERROR (-20000,'SUBIDA EXCESIVA NO DEJO');
			  
			  END SALARIOS;
			  
			
			
		-- QUIEN TENGA COMISION NO SE PUEDE BAJAR LA COMISION 
		-- ANTE NUEVAS COMISIONES(NUEVOS EMPLEADOS), COMISION ES OBLIGATORIA SI ES DEL DEPT_NO 30. COMO MUCHO COMOSION DE 200.000
		-- SI ES DEL 20, NO ES OBLIGATORIA Y COMISION DE 100.000
		
		CREATE OR REPLACE TRIGGER 
			CONTROLACOMISION
			BEFORE UPDATE OR INSERT OF COMISION
			ON EMPLE
			FOR EACH ROW 
			DECLARE
			NO EXCEPTION;
			TEXTO VARCHAR2(100);  
			  BEGIN
			
			CASE :NEW.COMISION
				WHEN 30 THEN
					IF :NEW.COMISION IS NULL THEN 
					
					END IF;
				
				WHEN 20 THEN
		
		
			 EXCEPTION 
				 WHEN NO THEN 
				 RAISE_APPLICATION_ERROR(-20000,TEXTO);
			  END SALARIOS;
	
## G.16- *10-05-24*
		-- Ante un nuevo alumno, matricularle en la tabla notas en todas las asignaturas 
		
			CREATE OR REPLACE TRIGGER 
			       MATRICULAR 
			   BEFORE INSERT 
			   ON ALUMNOS
			   FOR EACH ROW
			   DECLARE
			   CURSOR C_CODASIG  IS SELECT COD,NOMBRE
			                       FROM ASIGNATURAS;
			   BEGIN
			     FOR V_COD IN C_CODASIG LOOP
			       INSERT INTO NOTAS(DNI,COD) VALUES(:NEW.DNI,V_COD.COD);
			     END LOOP;
			     
			   END MATRICULAR;
		
		-- nueva asignatura, matriculamos todos los alumnos en notas
		
		
		
		-- ante una nueva venta (tventas) comprobamos si tenemos unidades suficientes con tarticulos, la paramos o la dejamos si hay unidades suficientes (VER CLASE)
		
		
		-- De la tabla de notas, no se puede dar de baja un alumno que este matriculado en (una asignatura, por ejemplo FOL)
		
		

## G.17- *13-05-24*

	Venajas: tienen permanentemente (en tiempo real) una tabla permanentemente actualizada con datos de lo que se está haciendo.

	Una tabla para el total de empleados, suma de salarios por dept_no... 
	
	CREATE TABLE ESTADISTICA
    (DEPARTAMENTO NUMBER,
    CUANTOS NUMBER,
    SUMA NUMBER
    );
    
	los disparadores INSTEAD OF , solo para vistas materializadas y for each row, se hacen en luga de la operacion dml
	
	CREATE OR REPLACE VIEW OTRAVISTA AS SELECT DEPT_NO, SUM(SALARIO) AS SUMA
    FROM EMPLE GROUP BY DEPT_NO;

	-- BORRAMOS EL DEP 15
	
	DELETE FROM OTRAVISTA 
	    WHERE DEPT_NO = 15;-- no deja 
	
		
		CREATE OR REPLACE TRIGGER
	    PARAVISTA
	    INSTEAD OF DELETE 
	    ON OTRAVISTA 
	    FOR EACH ROW
	    DECLARE
	    BEGIN
	        DELETE FROM EMPLE 
	            WHERE DEPT_NO = :OLD.DEPT_NO; -- VENTAJA DE QUE AL USUARIO TIENE DERECHO SOBRE OTRAVISTA, NO SOBRE EMPLE
	    END PARAVISTA;
    
    
	- emp no, apellido, oficio, dnombre y loc 
	Queremos HACER ESTAS OPERACIONES: NO DEJA
	
	INSERT INTO OTRAVISTA2 VALUES(4444,'CUATROS','NOSE','BARCELONA','VENTAS');
	
	UPDATE OTRAVISTA2
	SET DNOMBRE = 'CONTABILIDAD'
	WHERE APELLIDO='GIL';


	CREATE OR REPLACE VIEW OTRAVISTA2 AS 
	SELECT EMP_NO,APELLIDO,OFICIO,DNOMBRE,LOC
	    FROM EMPLE E 
	    INNER JOIN DEPART D 
	    ON E.DEPT_NO=D.DEPT_NO;

	Hay que dar 2 obligatorios: emp_no (lo damos) y dept_no, de donde lo cojemos? mirando en depart, con dnombre,loc podemos sacarlo



	- tablas mutantes
	  la inmensa mayoria de los disparadores dan tablas mutantes. En un disparador for each row y update en el cuerpo del disparador no se puede acceder a los datos que han hecho motivar el salto del disparador. No se puede select into de la tabla porque está cambiando. 
	- faltan tablas mutantes clase18?
## G.18- *20-05-24*

	COMPOUND TRIGGERS: para no tener que acceder a tablas para obtener los parametros de otro trigger, podemos hacer un trigger compound.
	
	
		sintaxis: CREATE OR REPLACE TRIGGER 
	   -- NOMBRE DEL DISPARADOR COMPUESTOLIMITE
	  FOR UPDATE OF LIMITE OR DELETE
	      OR INSERT   
	  ON TABLA O VISTA
	  COMPOUND  TRIGGER
	   --ZONA DE DECLARACIONES GLOBALES
	   BEFORE STATEMENT IS
	     --ZONA LOCAL DEL EVENTO
	   BEGIN
	     NULL;
	   END BEFORE STATEMENT;
	  
	   AFTER STATEMENT IS
	     --ZONA LOCAL DEL EVENTO
	   BEGIN
	     NULL;
	   END AFTER STATEMENT; 
	   
	   BEFORE EACH ROW IS
	     --ZONA LOCAL DEL EVENTO
	   BEGIN
	     NULL;
	   END BEFORE EACH ROW;
	     
	    AFTER EACH ROW IS
	     --ZONA LOCAL DEL EVENTO
	   BEGIN
	     NULL;
	   END AFTER EACH ROW; 
	END COMPUESTOLIMITE;   
		
	los sub eventos son opcionales

	-- el de c18 de emple era control de salario * 2(avg)
	ante upd, o insertt, ninguno ^^ pueda ganar eso. Con 2 disparadores
	
	before statemtent puede acceder a la tabla que esta mutando
	each row da problema

	CON ARRAY

	-PAQUETES (cabecera y body) para usar en sesion
	body opcional, con cabecera tiene sentido.
	cabecera -> es publico todo lo que declare y mantiene los valores hasta que el user se desconecte.
	el body es obligatorio cuando en ponemos procedimientos o fucniones en la cabecera del paquete. 
	con dos disparadores vamos a acceder el package
	
	CREATE OR REPLACE PACKAGE 
	PARAEMPLE
	AS 
	DATO NUMBER;
	
	END PARAEMPLE;
	
	hacer paraemple con packages


## G.19- *23-05-24*
	PROCE O FUNC HAY QUE HACER BODY O NO VA

	mismo nombre proce en cabecera y body, los atributos y nombres de los atrib
	
	
	CREATE OR REPLACE PACKAGE PEMPLE1 
	AS
	    PROCEDURE INSERTAR(NOM IN EMPLE.APELLIDO%TYPE, NUM NUMBER,
	                      SAL IN NUMBER, DEPT IN NUMBER);
	                      
	    FUNCTION BUSCAR (NOM IN VARCHAR2) 
	            RETURN BOOLEAN;
	            
	    PROCEDURE BORRAR (NUM IN NUMBER);
	    
	    TYPE MIREGISTRO IS RECORD
	        (NOM VARCHAR2(40),
	        NUM NUMBER);
	    
	    TYPE MITABLA IS TABLE OF MIREGISTRO 
	        INDEX BY BINARY_INTEGER; -- importante
	    
	    NO EXCEPTION;
	    
	    PROCEDURE LISTAR(DEP IN NUMBER,VECTOR IN OUT MITABLA);
	    
	
	END PEMPLE1;
	
	
	CREATE OR REPLACE PACKAGE BODY PEMPLE1
	AS -- SOLAMENTE DENTOR DEL PAQUETE, PROTECTED
	    PROCEDURE INSERTAR(NOM IN EMPLE.APELLIDO%TYPE, NUM NUMBER,
	                      SAL IN NUMBER, DEPT IN NUMBER)
	    AS
	    
	    BEGIN
	        INSERT INTO EMPLE(EMP_NO,APELLIDO,SALARIO,DEPT_NO)
	        VALUES (NUM,NOM,SAL,DEPT);
	        COMMIT WORK;
	        DBMS_OUTPUT.PUT_LINE('INSERTADO');
	    END INSERTAR;
	                      
	    FUNCTION BUSCAR (NOM IN VARCHAR2) 
	            RETURN BOOLEAN
	    AS
	    REG EMPLE%ROWTYPE;
	    BEGIN
	        SELECT * INTO REG
	            FROM EMPLE
	            WHERE APELLIDO = NOM;
	        DBMS_OUTPUT.PUT_LINE('NUMERO. ' || REG.EMP_NO);
	        RETURN TRUE;
	        
	        EXCEPTION
	        WHEN NO_DATA_FOUND THEN 
	          DBMS_OUTPUT.PUT_LINE('NO ESTA');
	          RETURN FALSE;
	    END BUSCAR;
	    
	    PROCEDURE LISTAR(DEP IN NUMBER,VECTOR IN OUT MITABLA)
	    AS
	    I PLS_INTEGER DEFAULT 1;
	    BEGIN 
	        FOR REG IN (SELECT APELLIDO,EMP_NO
	                    FROM EMPLE
	                    WHERE DEPT_NO = DEP) LOOP
	            vector(I):= REG;
	            I:= I+1;
	        END LOOP;
	    END LISTAR;
	    
	    PROCEDURE BORRAR (NUM IN NUMBER)
	    AS 
	    BEGIN
	        DELETE FROM EMPLE WHERE NUM = EMP_NO;
	          IF SQL%NOTFOUND THEN
	                RAISE NO;
	            END IF;
	        DBMS_OUTPUT.PUT_LINE('BORRADO');
	        
	        EXCEPTION 
	            WHEN NO THEN
	            DBMS_OUTPUT.PUT_LINE('NO ESTA');
	    END BORRAR;
	
	END PEMPLE1;
	
	
	ALTER TABLE EMPLE DISABLE ALL TRIGGERS;
	
	BEGIN
	    PEMPLE1.insertar(NOM => 'PESADO', NUM => 2323,SAL =>200,DEPT=>30);
	    
	END;
	
	BEGIN
	    PEMPLE1.BORRAR(2323);
	
	END;
	
	
	CREATE OR REPLACE PROCEDURE LLAMOPAQUETE(DEP IN NUMBER)
	AS
	    VEC PEMPLE1.MITABLA;
	BEGIN
	    PEMPLE1.LISTAR(VECTOR => VEC, DEP => 20);
	    FOR I IN 1..VEC.COUNT LOOP
	        DBMS_OUTPUT.PUT ('EMPLEADO ' || VEC(I).NOM);
	        DBMS_OUTPUT.PUT_LINE ('--NUMERO ' || VEC(I).NUM);
	    END LOOP;
	END;
	
	BEGIN
	    LLAMOPAQUETE(20);
	END;
	
	
	-- TUBERIAS




# H- Apuntes limpios
## H.1- Introduccion
	bloques de PL 
		tipos:
		 - No almacenados: (codigo no se guarda en la BB.DD)
				 - anónimos: no tienen nombre
				 - nominados: tienen nombre 
			 - Almacenados: se almacenan en la BB.DD, todos tienen nombre
				 - disparadores (triggers)
				 - otros: 
					Simples , procedimientos (void)
					funciones (retorna algo? función)
					Paquetes (agrupan funciones) 2 partes: cabecera y cuerpo.  Hasta log out los valores son globales por sesión
					Tipos objeto (cabecera / cuerpo)
	tipos de datos DE PL SOLO:
		- añadimos los de PL : boolean (true,false,null), binary_integer(MIERDA), pls.integer (MEJOR);
		number usa (bcd) y pls_integer usa (bp) - mejor calcular con bp 
		Atributos de tablas -> number   .  Para operar en memoria -> pls_integer
		varchar2 -> limite 32k
			

## H.2- Bloques anonimos 
		si no se inicializan las variables, valen null 
## H.3- IN OUT

1. **IN**: Se utiliza para pasar valores a un procedimiento. Cuando se declara un parámetro como IN, significa que este parámetro solo puede ser utilizado para leer el valor dentro del procedimiento, pero no se puede modificar. En el ejemplo, `P_IN` es un parámetro de entrada y se le puede asignar un valor predeterminado utilizando `DEFAULT`.

2. **OUT**: Se utiliza para devolver un valor desde el procedimiento. Cuando se declara un parámetro como OUT, significa que este parámetro solo se puede utilizar para escribir un valor dentro del procedimiento, pero no se puede leer. En el ejemplo, `P_OUT` es un parámetro de salida y no se le puede asignar un valor predeterminado.

3. **IN OUT**: Se utiliza para pasar un valor al procedimiento y también para devolver un valor modificado desde el procedimiento. Cuando se declara un parámetro como IN OUT, significa que este parámetro se puede utilizar tanto para leer como para escribir dentro del procedimiento. En el ejemplo, `P_DOS` es un parámetro de entrada/salida.

En el código del ejemplo, se destacan las siguientes líneas:

- `P_IN:=10;` -- Esta línea intentará asignar el valor 10 al parámetro `P_IN`, pero el compilador arrojará un error porque no se puede asignar un valor a un parámetro IN.
- `P_OUT:=20;` -- Esta línea asigna el valor 20 al parámetro `P_OUT`, lo cual está permitido porque `P_OUT` es un parámetro OUT.
- `NORMALIN:= P_OUT;` -- Esta línea intentará asignar el valor de `P_OUT` a la variable `NORMALIN`, pero arrojará un error porque un parámetro OUT solo puede ser utilizado para escribir valores, no para leer.
- `NORMALIN:=10;` -- Esta línea asigna el valor 10 a la variable `NORMALIN`, lo cual está permitido porque `NORMALIN` es una variable normal.

En resumen, los parámetros IN se utilizan para pasar valores a un procedimiento, los parámetros OUT se utilizan para devolver valores desde un procedimiento, y los parámetros IN OUT se utilizan para pasar valores al procedimiento y también para devolver valores modificados desde el procedimiento.

## H.4- FUNCIONES DETERMINISTICAS 

