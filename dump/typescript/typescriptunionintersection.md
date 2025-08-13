# TYPESCRIPT -> UNION & INTERSECTION
## SUMMARY

**Official:**  
> Los tipos de unión (`Union`) y de intersección (`Intersection`) en TypeScript permiten combinar múltiples tipos para ampliar o restringir las posibilidades de los valores que una variable puede tener.

**Personal:**  
> Un **Union Type** es como decir "puede ser uno u otro".  
> Un **Intersection Type** combina tipos para decir "debe cumplir con ambos".  
> Son herramientas clave para manejar datos flexibles y estructurados.

---

## THEORY

### UNION TYPES (`|`)
Permiten que una variable acepte múltiples tipos de datos. Son útiles cuando un valor puede ser de más de un tipo.

#### EJEMPLO BÁSICO:
>[!info]- **Uso de Union Types**  
> ```typescript
> let id: string | number;
> id = 123;  // Válido
> id = "ABC"; // También válido
> ```

#### USO EN FUNCIONES:
Se pueden usar para manejar parámetros con diferentes tipos.
>[!info]- **Union Types en funciones**  
> ```typescript
> function imprimirId(id: string | number): void {
>   if (typeof id === "string") {
>     console.log("El ID es un texto:", id);
>   } else {
>     console.log("El ID es un número:", id);
>   }
> }
> imprimirId(123); // Salida: El ID es un número: 123
> imprimirId("ABC"); // Salida: El ID es un texto: ABC
> ```

---

### INTERSECTION TYPES (`&`)
Permiten combinar múltiples tipos, exigiendo que una variable cumpla con las características de todos ellos.

#### EJEMPLO BÁSICO:
>[!info]- **Uso de Intersection Types**  
> ```typescript
> type Persona = { nombre: string; };
> type Empleado = { salario: number; };
> type EmpleadoPersona = Persona & Empleado;
> 
> let trabajador: EmpleadoPersona = { nombre: "Diego", salario: 3000 };
> ```

#### USO EN ESTRUCTURAS COMPLEJAS:
>[!info]- **Intersection Types con interfaces complejas**  
> ```typescript
> interface Usuario {
>   id: number;
>   nombre: string;
> }
> interface Admin {
>   permisos: string[];
> }
> 
> type UsuarioAdmin = Usuario & Admin;
> 
> const admin: UsuarioAdmin = {
>   id: 1,
>   nombre: "Diego",
>   permisos: ["lectura", "escritura"]
> };
> ```

---

## QUESTIONS

>[!tip]- **¿Qué es un Union Type en TypeScript?**  
> Es una combinación de tipos que permite que una variable tenga más de un tipo posible.  
> ```typescript
> let valor: string | number;
> valor = "Texto"; // Válido
> valor = 42;      // También válido
> ```

>[!question]- **¿Qué diferencia hay entre Union y Intersection Types?**  
> - **Union (`|`)**: Una variable puede ser de uno u otro tipo.  
> - **Intersection (`&`)**: Una variable debe cumplir con ambos tipos.  
> ```typescript
> type A = { a: string };
> type B = { b: number };
> let union: A | B = { a: "Texto" }; // Correcto con A
> let interseccion: A & B = { a: "Texto", b: 42 }; // Correcto con ambos
> ```

>[!warning]- **¿Qué sucede si usas propiedades que no están en todos los tipos en un Union?**  
> TypeScript solo permite acceder a las propiedades comunes entre los tipos.  
> ```typescript
> type A = { a: string };
> type B = { b: number };
> let union: A | B = { a: "Texto" };
> console.log(union.a); // OK
> console.log(union.b); // Error
> ```

>[!danger]- **¿Cuándo usar Intersection Types sobre Union Types?**  
> Usa Intersection Types cuando necesites una combinación estricta de tipos (ambos deben cumplirse).  
> ```typescript
> type Persona = { nombre: string; };
> type Empleado = { salario: number; };
> let trabajador: Persona & Empleado = { nombre: "Ana", salario: 4000 }; // Necesita ambas propiedades
> ```
- - - 
#typescript 