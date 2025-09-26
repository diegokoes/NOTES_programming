# JAVA -> OOP -> INTERFACES
## SUMMARY
> [!summary]
> 
- - - 
## THEORY

### TYPES OF INTERFACES
- [Functional Intefaces](javafunctionalinterfaces.md)
-  [Marker Intefaces](java_markerinterfaces.md)
- [General Interfaces](javainterfaces.md)
- [Extending Interfaces](javaextendinginterfaces.md)

- **Interfaces Funcionales:**
    
    - Simplifican el código mediante expresiones lambda.
    - Son clave para la programación funcional.
- **Interfaces de Marcador:**
    
    - Facilitan la integración con sistemas como serialización o clonación.
- **Interfaces Generales:**
    
    - Promueven la reutilización y desacoplamiento del código.
- **Interfaces Extensibles:**
    
    - Permiten construir jerarquías complejas de comportamientos.
- **Interfaces Especializadas:**
    
    - Solucionan problemas específicos, como el ordenamiento (`Comparable`) o la concurrencia (`Runnable`).

### TYPES OF METHODS IN INTERFACES

1. **Métodos Abstractos:**
    - Son métodos que no tienen cuerpo y deben ser implementados por la clase que implementa la interfaz.
    - **Ejemplo:**
    ```java
    public interface Animal {
        void hacerSonido(); // Método abstracto
    }
    ```


---

2. **Métodos Default (Desde Java 8):**
    - Son métodos con cuerpo que las interfaces pueden incluir.
    - Útil para añadir comportamiento común sin romper compatibilidad con implementaciones existentes.
    - **Ejemplo:**
    ```java
    public interface Animal {
        default void dormir() {
            System.out.println("El animal está durmiendo");
        }
    }
    ```

---

1. **Métodos Estáticos (Desde Java 8):**
    - Métodos que pertenecen a la interfaz y no pueden ser sobrescritos.
    - Se llaman directamente desde la interfaz, no desde una instancia.
    - **Ejemplo:**
    ```java
    public interface Utilidad {
        static void imprimirMensaje(String mensaje) {
            System.out.println(mensaje);
        }
    }
    ```


---

2. **Métodos Privados (Desde Java 9):**
    - Métodos auxiliares dentro de la interfaz que no son visibles fuera de ella.
    - Solo pueden ser utilizados por otros métodos dentro de la interfaz (como `default` o `static`).
    - **Ejemplo:**
    ```java
    public interface Animal {
        default void comer() {
            masticar();
        }
        private void masticar() {
            System.out.println("Masticando...");
        }
    }
    ```

---

### EXAMPLES

### E.1.1- **EJEMPLO COMPLETO:**
```java
public interface Vehiculo {
    // Método abstracto
    void acelerar();

    // Método default
    default void frenar() {
        System.out.println("El vehículo está frenando.");
    }

    // Método estático
    static void revisar() {
        System.out.println("El vehículo está en revisión.");
    }
}

public class Coche implements Vehiculo {
    @Override
    public void acelerar() {
        System.out.println("El coche está acelerando.");
    }

    public static void main(String[] args) {
        Coche coche = new Coche();
        coche.acelerar();
        coche.frenar();

        // Llamada al método estático
        Vehiculo.revisar();
    }
}
```
## QUESTIONS

- > [!tip]- **¿Qué es una interfaz en Java?**
    Es una estructura que define un conjunto de métodos que una clase debe implementar. Una interfaz puede contener métodos abstractos y métodos con implementación (a partir de Java 8).

- > [!question]- **¿Qué diferencia hay entre una interfaz y una clase abstracta?**
    >- Las interfaces no pueden tener constructores, mientras que las clases abstractas sí.
    >- Una clase puede implementar múltiples interfaces, pero solo puede extender una clase abstracta.

- > [!warning]- **¿Qué sucede si una clase no implementa todos los métodos de una interfaz?**
    La clase debe declararse como abstracta, ya que no cumple completamente el contrato de la interfaz.

- > [!danger]- **¿Cómo manejarías conflictos entre métodos por implementar en múltiples interfaces?**
    Si dos interfaces tienen métodos con la misma firma, la clase debe implementar explícitamente la lógica o especificar cuál método usar mediante `InterfaceName.super.metodo()`.

- > [!tip]- **¿Qué tipos de métodos puede haber en una interfaz en Java?**
    Las interfaces pueden contener métodos abstractos, predeterminados (`default`), estáticos (`static`) y privados (`private`). Consulta los apartados detallados más adelante.

- - - 
[[poo_java]] [[clases_abstractas]] ava