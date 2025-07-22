# Java: Extending Interfaces
## Definition
**Official:**
> Son interfaces diseñadas para ser extendidas por otras interfaces, creando jerarquías de interfaces.

**Personal:**
>Estas interfaces sirven como base para otras interfaces, permitiendo construir estructuras complejas y reutilizables.

### A.1.1- **Características:**

- Permiten extender múltiples interfaces al mismo tiempo.
- Facilitan la organización y expansión de código.
```java
public interface Vehiculo {
    void mover();
}

public interface VehiculoTerrestre extends Vehiculo {
    void frenar();
}
```
- - - 
