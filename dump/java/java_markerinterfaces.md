# java -> interfaces -> marker I.
## Summary
> [!summary]-
> 
- - - 

## Definition
**Official:**
> Las **interfaces de marcador** (marker interfaces) son interfaces vacías, es decir, no contienen métodos ni campos. Se utilizan para marcar o identificar una clase con un comportamiento especial.

**Personal:**
>Son interfaces que no hacen nada por sí mismas, pero le indican al sistema que una clase cumple un propósito especial.

### A.1.1- **Características:**

- No definen métodos ni comportamiento.
- Se usan como "banderas" para que el sistema reconozca algo especial sobre una clase.
- - - 
## Examples
En este caso, `Serializable` es una interfaz de marcador que indica que `MiClase` puede ser serializada.
```java
public class MiClase implements Serializable {
    private int id;
    private String nombre;
}
```
