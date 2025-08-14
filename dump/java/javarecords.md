# JAVA -> OOP / DATA TYPES -> RECORDS

A continuación te presento unos apuntes claros y organizados sobre los **Records en Java**, incluyendo los puntos principales (inmutabilidad, constructores, métodos generados, validaciones, uso de `static`, asociaciones, herencia, etc.) y conectándolos con los ejemplos de código proporcionados.

---

## 1. ¿QUÉ SON LOS RECORDS EN JAVA?

Los _records_ son un tipo especial de clase introducido formalmente en Java 16 (con vista previa desde Java 14) que se utilizan para **modelar datos inmutables**. Un record:

- Declara sus componentes (atributos) de manera **compacta**.
- Todos sus campos son por definición `private` y `final`.
- Dispone de un **constructor público** con todos esos atributos.
- Ofrece automáticamente métodos `equals`, `hashCode` y `toString`.
- Proporciona **getters** (en forma de métodos con el mismo nombre de los campos).
- No puede heredarse (los records son efectivamente `final`).

### ¿POR QUÉ USAR RECORDS?

Antes de los records, era común crear clases _DTO_ (Data Transfer Objects) para transportar datos, lo que conllevaba mucho código repetitivo (constructores, getters, setters, `equals`, `hashCode`, `toString`, etc.). Los records **reducen drásticamente** esta sobrecarga:

```java
public record Alumno(String nombre, String apellidos, String email, int edad) { }
```

Con solo esta línea, tenemos:

- Campos inmutables `nombre, apellidos, email, edad`.
- Constructor canónico (con todos los campos).
- Métodos `nombre()`, `apellidos()`, `email()`, `edad()` que actúan como getters.
- Métodos `equals(...)`, `hashCode()` y `toString()` generados automáticamente.

---

## 2. CONSTRUCTORES EN RECORDS

### 2.1. CONSTRUCTOR CANÓNICO

Por defecto, Java genera un **constructor canónico** que recibe todos los campos del record en el orden en que se declaran. En el ejemplo:

```java
public record Alumno(String nombre, String apellidos, String email, int edad) { }
```

Java genera automáticamente:

```java
public Alumno(String nombre, String apellidos, String email, int edad) {
    this.nombre = nombre;
    this.apellidos = apellidos;
    this.email = email;
    this.edad = edad;
}
```

### 2.2. CONSTRUCTOR COMPACTO (O BLOQUE DE CONSTRUCCIÓN)

Podemos “personalizar” la construcción con validaciones o transformaciones. Esto se hace definiendo un **constructor compacto**, sin parámetros explícitos, que internamente usa los mismos nombres de los componentes:

```java
public record Alumno(String nombre, String apellidos, String email, int edad) {

    public Alumno {
        // Validaciones básicas
        Objects.requireNonNull(nombre);
        Objects.requireNonNull(apellidos);
        // Si no cumple, podemos lanzar excepción
        // ...
    }
    
    // ...
}
```

- Este bloque se ejecuta **después** de que Java haya asignado los valores a los campos.
- Sirve, por ejemplo, para chequear que no vengan valores nulos o aplicar transformaciones de texto.

### 2.3. CONSTRUCTORES ADICIONALES

Además del constructor canónico y el compacto, puedes crear **otros constructores** que deleguen al principal. Ejemplo:

```java
public record Alumno(String nombre, String apellidos, String email, int edad) {
    
    public Alumno {
        // Validaciones/transformaciones
        Objects.requireNonNull(nombre);
        Objects.requireNonNull(apellidos);
    }
    
    // Constructor adicional con menos parámetros
    public Alumno(String nombre, String apellidos, int edad) {
        this(
            nombre,
            apellidos,
            nombre.toLowerCase() + "." + apellidos.toLowerCase() + "@ow.net",
            edad
        );
    }
}
```

Este enfoque te permite **establecer valores por defecto** o calcular algún valor de un campo (como un email en función del nombre y apellidos).

---

## 3. INMUTABILIDAD

Los records están **pensados para ser inmutables**. Los campos son `private final` y no puedes modificar sus valores después de la construcción. Por ello, resultan muy útiles para transportar datos de forma segura entre capas de la aplicación.

---

## 4. MÉTODOS `static` Y CAMPOS `static`

Los records pueden incluir:

- **Métodos estáticos** (`static`) de creación o utilidad.
- **Campos estáticos**.

Por ejemplo:

```java
public record Alumno(String nombre, String apellidos, String email, int edad) {
    
    public static final String DOMINIO = "@openwebinars.net";
    
    // Ejemplo de método estático de factoría:
    public static Alumno of(Persona p) {
        // Cálculo de edad a partir de fecha de nacimiento
        int edadCalculada = (int) ChronoUnit.YEARS.between(p.getFechaNacimiento(), LocalDate.now());
        return new Alumno(p.getNombre(), p.getApellidos(), edadCalculada);
    }
    
    // ...
}
```

En este ejemplo, `DOMINIO` es una **constante** estática y `of(Persona p)` es un **método estático** que construye un `Alumno` a partir de una `Persona`.

---

## 5. ASOCIACIONES ENTRE RECORDS

Al igual que las clases, un record puede **contener** otros objetos, incluidos otros records. Por ejemplo, un record `Empresa` que contenga un array de `Empleado`:

```java
public record Empresa(String nombre, Empleado[] empleados) { }
public record Empleado(String nombre, String apellidos, LocalDate fechaContratacion) { }
```

Esta relación se ve en el ejemplo:

```java
Empresa emp = new Empresa("Microsoft", new Empleado[] {
    new Empleado("Bill", "Gates", LocalDate.of(1975, 4, 4)),
    new Empleado("Paul", "Allen", LocalDate.of(1975, 4, 4))
});
```

- Aquí `Empresa` **agrega o compone** una lista (array) de `Empleado`.
- Se aplican las mismas reglas de inmutabilidad para los componentes de un record; sin embargo, _el array en sí no es inmutable_ (si se quisiera, se podrían envolver los datos en una colección inmutable o realizar copias defensivas).

---

## 6. HERENCIA EN RECORDS

Los records son, efectivamente, **finales**. No pueden:

- Extender otra clase (salvo la implícita `java.lang.Record`).
- Ser extendidos por otra clase o record.

Sin embargo, **sí pueden implementar interfaces**:

```java
public record Empleado(String nombre, String apellidos, LocalDate fechaContratacion)
       implements Nombrable {
    
    @Override
    public String nombreCompleto() {
        return nombre + " " + apellidos;
    }
}
```

En el ejemplo:

```java
public interface Nombrable {
    String nombreCompleto();
}
```

- `Empleado` (que es un record) **implementa** `Nombrable`.
- Así, hereda la firma (y posibles métodos `default`) de la interfaz.

---

## 7. EJEMPLOS DESTACADOS

A continuación, resumimos lo que ilustran los ejemplos principales:

1. **Ejemplo 1**
    
    ```java
    public record Alumno(String nombre, String apellidos, String email, int edad) { }
    ```
    
    - Muestra la **declaración básica** de un record, el uso del constructor canónico, el `toString`, `equals` y `hashCode` automáticos.
    - Compara dos instancias (`a.equals(b)` vs `a.equals(c)`) para mostrar la importancia de los valores de los campos en la igualdad.
2. **Ejemplo 2**
    
    ```java
    public record Alumno(String nombre, String apellidos, String email, int edad) {
        public Alumno {
            Objects.requireNonNull(nombre);
            Objects.requireNonNull(apellidos);
        }
        public Alumno(String nombre, String apellidos, int edad) {
            this(nombre, apellidos, nombre.toLowerCase() + "." + apellidos.toLowerCase() + "@ow.net", edad);
        }
        public String nombreCompleto() {
            return this.nombre + " " + this.apellidos;
        }
    }
    ```
    
    - Uso del **constructor compacto** para validaciones (`Objects.requireNonNull`).
    - Creación de **constructores adicionales** con lógica específica (generación de `email`).
    - Definición de un **método adicional** (`nombreCompleto`).
    
    También se ve cómo se puede personalizar otro record `Producto` aplicando transformaciones (por ejemplo, `nombre.trim().toUpperCase()`).
    
3. **Ejemplo 3**
    
    - Incluye un **campo estático** `DOMINIO`.
    - Un **método estático** `of(Persona p)` que convierte una `Persona` en un `Alumno`.
    - Demuestra que un record puede recibir objetos de otra clase (`Persona`) y trabajar con ellos.
4. **Ejemplo 4**
    
    - Muestra **asociaciones** con un record `Empresa` que tiene un array de `Empleado`.
    - Ilustra cómo en el `toString` puede personalizarse la forma de imprimir el array (`Arrays.toString`).
5. **Ejemplo 5**
    
    - Enseña cómo **un record implementa una interfaz** (`Nombrable`).
    - Ejemplifica la imposibilidad de heredar de otro record o clase, pero sí **implementar métodos** de una interfaz.

---

## 8. CONCLUSIONES

- Los records simplifican drásticamente la creación de **objetos inmutables**.
- Incluyen un **constructor canónico** y generan automáticamente `equals`, `hashCode` y `toString`.
- Pueden contener **constructores adicionales**, **constructores compactos** (para validaciones) y **métodos estáticos**.
- Soportan **asociaciones** con otros records o clases (permitiendo componer datos).
- No soportan **herencia** (son finales) pero **pueden implementar** interfaces.
- Son ideales para **transportar datos** (DTOs) y mantener la inmutabilidad en un diseño orientado a objetos.

Con esto, dispones de una **visión completa** de cómo funcionan los records en Java y de cómo cada ejemplo ilustra conceptos clave: constructores, métodos estáticos, asociaciones, validaciones, herencia con interfaces, etc. ¡Te servirán para simplificar y hacer más segura la definición de estructuras de datos inmutables en tus proyectos Java!

- - - 
ava  odo 