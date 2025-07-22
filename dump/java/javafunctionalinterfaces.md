# Java: Functional Interfaces
## Definition
**Official:**
> Una **interfaz funcional** es una interfaz que tiene exactamente un método abstracto. Estas interfaces son la base para las expresiones lambda y las referencias a métodos en Java.

**Personal:**
>Es una interfaz que define una única acción que debe ser implementada. Son ideales para simplificar tareas repetitivas con expresiones lambda.

```java
@FunctionalInterface
public interface Operacion {
    int operar(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        Operacion suma = (a, b) -> a + b;
        System.out.println(suma.operar(3, 5)); // 8
    }
}
```
 **Características:**

- Contienen un único método abstracto.
- Pueden tener métodos `default` y `static`.
- Usan la anotación `@FunctionalInterface` para asegurar que cumplen con esta regla.
- - - 
## Questions 
> [!info]- Click to reveal the answer
JavaScript is a programming language used primarily for web development.

