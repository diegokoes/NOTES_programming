# JAVA -> OOP / DATA TYPES -> ENUMS
## DEFINICIÓN Y CONCEPTO FUNDAMENTAL

Las enumeraciones o `enum` son un tipo especial de clase en Java que representan un conjunto cerrado y predefinido de valores constantes. Podemos entenderlas de dos maneras:

1. Como un conjunto cerrado de valores simples
2. En un concepto más avanzado, como un conjunto cerrado de instancias de una clase

Las enumeraciones se declaran usando la palabra reservada `enum` y nos permiten definir tipos de datos que consisten en un conjunto fijo de constantes específicas.

## CARACTERÍSTICAS BÁSICAS DE LAS ENUMERACIONES

- Se definen mediante la palabra reservada `enum`
- Cada valor del enum es implícitamente `public`, `static` y `final`
- Los valores se separan por comas
- Por convención, los nombres de los valores se escriben en mayúsculas
- Cada enumeración es un tipo de dato independiente que mejora la seguridad de tipos

## MÉTODOS INHERENTES A TODAS LAS ENUMERACIONES

Cada tipo enum incluye automáticamente algunos métodos útiles:

- `values()`: Devuelve un array con todos los valores de la enumeración
- `valueOf(String)`: Convierte una cadena en el valor enum correspondiente
- `name()`: Devuelve el nombre del valor enum como una cadena
- `ordinal()`: Devuelve la posición (índice) del valor dentro de la enumeración

## ENUMERACIONES AVANZADAS

Las enumeraciones en Java van mucho más allá de simples constantes nombradas. Son auténticas clases con capacidades avanzadas:

### ATRIBUTOS Y CONSTRUCTORES

- Pueden tener atributos (campos) para almacenar información adicional
- Pueden tener constructores, aunque estos deben ser privados
- Los atributos suelen declararse como `final` para mantener la inmutabilidad

### MÉTODOS

- Pueden tener métodos de instancia y métodos estáticos
- Cada valor de la enumeración puede acceder a estos métodos
- Permiten encapsular comportamientos asociados a cada constante

### INTERFACES Y LIMITACIONES DE HERENCIA

- No pueden heredar de otras clases (ya heredan implícitamente de `java.lang.Enum`)
- Pueden implementar interfaces, lo que permite que los valores enum tengan comportamientos polimórficos
- Cada valor de la enumeración puede proporcionar implementaciones específicas para métodos abstractos

### SOBRESCRITURA DE MÉTODOS POR VALOR

- Cada valor individual de la enumeración puede sobrescribir métodos, proporcionando comportamientos específicos
- Esto permite que cada constante tenga su propia implementación de un método

## CASOS DE USO COMUNES

1. **Representación de conjuntos fijos de valores**:
   - Direcciones cardinales: NORTE, SUR, ESTE, OESTE
   - Movimientos de cursor: ARRIBA, ABAJO, IZQUIERDA, DERECHA
   - Meses del año: ENERO, FEBRERO, ..., DICIEMBRE
   - Días de la semana: LUNES, MARTES, ..., DOMINGO

2. **Estados en un flujo de proceso**:
   - Estados de un pedido: PEDIDO, PREPARADO, DELIVERED
   - Estados de una transacción: INICIADA, PROCESANDO, COMPLETADA, FALLIDA
   - Estados de un documento: BORRADOR, REVISIÓN, PUBLICADO, ARCHIVADO

3. **Opciones de configuración**:
   - Niveles de log: DEBUG, INFO, WARNING, ERROR
   - Tipos de ordenamiento: ASCENDENTE, DESCENDENTE
   - Formatos de archivo: PDF, DOC, TXT, XML

## ANÁLISIS DE LOS EJEMPLOS DE CÓDIGO

### EJEMPLO 1: ENUMERACIÓN SIMPLE (DIASEMANA)

```java
public enum DiaSemana {
    LUNES, MARTES, MIERCOLES, JUEVES, VIERNES, SABADO, DOMINGO
}
```

Esta enumeración representa un conjunto cerrado de constantes para los días de la semana. Es una implementación básica que aprovecha los métodos inherentes como `values()`, `name()` y `ordinal()`.

En la clase App, podemos ver cómo se recorren todos los valores mediante:

```java
for(DiaSemana dia : DiaSemana.values()) {
    System.out.println("%d. %s: %s".formatted(dia.ordinal(), dia.name(), saludoDia(dia)));
}
```

### EJEMPLO 2: ENUMERACIÓN AVANZADA (ESTADOPEDIDO)

```java
public enum EstadoPedido {
    PEDIDO("Pedido realizado", 0, 0),
    PREPARADO("Listo para entrega", 0, 2),
    DELIVERED("Entregado", 1, 5);
    
    private EstadoPedido(String descripcion, int plazoMinimo, int plazoMaximo) {
        this.descripcion = descripcion;
        this.plazoMinimo = plazoMinimo;
        this.plazoMaximo = plazoMaximo;
    }
    
    private final String descripcion;
    private final int plazoMinimo;
    private final int plazoMaximo;
    
    public String getDescripcion() {
        return descripcion;
    }
    
    public int getPlazoMinimo() {
        return plazoMinimo;
    }
    
    public int getPlazoMaximo() {
        return plazoMaximo;
    }
}
```

Esta enumeración demuestra características avanzadas:
- Cada constante incluye datos adicionales (descripción y plazos)
- Tiene un constructor privado para inicializar estos valores
- Define atributos privados y finales
- Proporciona métodos getter para acceder a los atributos

## VENTAJAS DE USAR ENUMERACIONES

1. **Seguridad de tipos**: El compilador verifica que solo se utilicen los valores definidos, evitando errores en tiempo de ejecución.
2. **Legibilidad del código**: Los nombres descriptivos de las constantes hacen el código más comprensible.
3. **Mantenibilidad**: Centraliza la definición de un conjunto de valores relacionados.
4. **Potencia expresiva**: Permite asociar comportamientos y datos a cada constante.
5. **Compatibilidad con switch**: Se integran perfectamente con expresiones y sentencias switch.

## USO CON ESTRUCTURAS DE CONTROL

Las enumeraciones son particularmente útiles con las estructuras `switch`, como se ve en el método `saludoDia`:

```java
public static String saludoDia(DiaSemana dia) {
    String msg = switch (dia) {
        case LUNES -> "Los lunes son duros :(";
        case JUEVES -> "Ya se acerca el finde :O";
        case VIERNES, SABADO -> "Los findes son los mejores días XDDDD";
        case DOMINGO -> "Ufff, que pereza";
        default -> "Ni fu, ni fa";
    };
    
    return msg;
}
```

Este código aprovecha la sintaxis moderna de `switch` con expresiones lambda y permite agrupar casos (como VIERNES y SÁBADO).

## COMPARACIÓN ENTRE ENUMERACIONES SIMPLES Y AVANZADAS

| Característica | Enumeración Simple | Enumeración Avanzada |
|----------------|--------------------|-----------------------|
| Definición | Solo nombres de constantes | Nombres, atributos y métodos |
| Constructores | No necesarios | Privados y obligatorios |
| Datos | No almacena | Puede almacenar datos específicos por constante |
| Comportamiento | Uniforme | Puede variar entre constantes |
| Uso típico | Conjuntos simples | Entidades con datos y comportamiento |

## CONSIDERACIONES DE DISEÑO

1. **Inmutabilidad**: Es una buena práctica que los enums sean inmutables, declarando sus campos como `final`.
2. **Singleton**: Cada constante de enum es efectivamente un singleton, garantizando una única instancia.
3. **Serialización**: Los enums tienen un mecanismo especial de serialización que preserva su singularidad.
4. **Rendimiento**: Las comparaciones entre valores enum son muy eficientes (comparación de referencias).
5. **Extensibilidad**: Si se anticipa la necesidad de agregar nuevas constantes frecuentemente, quizás un enum no sea la mejor opción.

## CONCLUSIÓN

Las enumeraciones en Java son una poderosa herramienta que va mucho más allá de simples constantes. Permiten definir tipos seguros con un conjunto cerrado de valores posibles, que pueden tener atributos y comportamientos específicos. Su uso adecuado mejora la legibilidad, la seguridad y la mantenibilidad del código, especialmente en situaciones donde tenemos un conjunto fijo y conocido de opciones o estados.
## SUMMARY
> [!summary]
> 
- - - 
## THEORY

## QUESTIONS
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer
- - - 
