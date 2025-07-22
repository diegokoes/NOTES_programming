# TypeScript -> Literal Types
## Summary
> [!summary]-
> 
- - - 

## Definition

**Official:**  
> Los **Literal Types** en TypeScript permiten restringir el valor de una variable a un conjunto específico de valores, como cadenas, números o booleanos literales. Son útiles para garantizar que solo valores específicos sean utilizados.

**Personal:**  
> Los Literal Types son etiquetas que limitan una variable a ciertos valores predefinidos. Combinados con uniones o estructuras más complejas, permiten crear aplicaciones más seguras y predecibles.

---

## Theory

## B.1- ¿Qué son los Literal Types?

Los tipos literales permiten definir valores específicos que una variable puede aceptar. Esto puede incluir cadenas, números y booleanos específicos.  

>[!info]- **Ejemplo Básico**  
>```typescript
>let estado: "activo" | "inactivo";
>estado = "activo";     // Válido
>estado = "inactivo";   // Válido
>estado = "pendiente";  // Error: no forma parte de los valores permitidos
>```

---

## B.2- Literales y Declaraciones de Variables

### Variables declaradas con `const`  
Las variables declaradas con `const` tienen un tipo literal, ya que sus valores no pueden cambiar.  

>[!info]- **Ejemplo con `const`**  
>```typescript
>const constante = "Hola Mundo";
>// constante tiene el tipo literal: "Hola Mundo"
>```

### Variables declaradas con `let`  
Las variables declaradas con `let` pueden cambiar su valor, por lo que el tipo será más general (`string` en lugar de un literal).  

>[!info]- **Ejemplo con `let`**  
>```typescript
>let variable = "Hola Mundo";
>variable = "Adiós Mundo"; // Válido, ya que el tipo es `string`
>```

---

## B.3- Combinaciones con [[typescriptunionintersection|Union Types]]

Los tipos literales son especialmente útiles cuando se combinan con **Union Types**, lo que permite limitar los valores aceptados.

>[!info]- **Ejemplo en Funciones**  
>```typescript
>function imprimirTexto(
>  texto: string, 
>  alineacion: "izquierda" | "derecha" | "centrada"
>): void {
>  console.log(`Texto: ${texto}, Alineación: ${alineacion}`);
>}
>
>imprimirTexto("Hola", "izquierda");  // Válido
>imprimirTexto("Adiós", "arriba");    // Error
>```

---

## B.4- Numeric Literal Types

Los números literales funcionan de manera similar a las cadenas, limitando los valores numéricos permitidos.

>[!info]- **Ejemplo con Literales Numéricos**  
>```typescript
>type Comparacion = -1 | 0 | 1;
>
>function comparar(a: string, b: string): Comparacion {
>  return a === b ? 0 : a > b ? 1 : -1;
>}
>
>const resultado = comparar("a", "b"); // Resultado: 1 o -1
>```

---

## B.5- Literales Booleanos

Los booleanos literales (`true` y `false`) también son válidos.

>[!info]- **Ejemplo con Booleanos Literales**  
>```typescript
>type Activacion = true | false;
>
>let activo: Activacion = true;   // Válido
>activo = false;                 // Válido
>activo = "true";                // Error
>```

---

## B.6- Literal Inference y `as const`

Cuando se inicializa un objeto, TypeScript infiere tipos generales (como `string` en lugar de `"GET"`). Esto puede modificarse utilizando **type assertions** o **`as const`**.

### Cambiar la Inferencia con `as const`
>[!info]- **Ejemplo con `as const`**  
>```typescript
>const solicitud = { url: "https://example.com", metodo: "GET" } as const;
>
>function manejarSolicitud(url: string, metodo: "GET" | "POST") {
>  console.log(`URL: ${url}, Método: ${metodo}`);
>}
>
>manejarSolicitud(solicitud.url, solicitud.metodo); // Válido
>```

### Cambiar la Inferencia con Type Assertions
>[!info]- **Ejemplo con Type Assertions**  
>```typescript
>const solicitud = { url: "https://example.com", metodo: "GET" as "GET" };
>
>function manejarSolicitud(url: string, metodo: "GET" | "POST") {
>  console.log(`URL: ${url}, Método: ${metodo}`);
>}
>
>manejarSolicitud(solicitud.url, solicitud.metodo); // Válido
>```

---

## Questions

>[!tip]- **¿Qué son los Literal Types en TypeScript?**  
> Son tipos que limitan una variable a un conjunto específico de valores permitidos.  
>```typescript
>let estado: "activo" | "inactivo";
>estado = "activo"; // OK
>estado = "pendiente"; // Error
>```

>[!question]- **¿Cómo se combinan los Literal Types con Union Types?**  
> Permiten limitar los valores aceptados en funciones o variables.  
>```typescript
>function mover(direccion: "izquierda" | "derecha") {
>  console.log(`Moviendo hacia: ${direccion}`);
>}
>mover("izquierda"); // OK
>mover("arriba");    // Error
>```

>[!warning]- **¿Qué sucede con la inferencia en objetos inicializados?**  
> TypeScript infiere tipos generales (`string`, `number`) en lugar de tipos literales. Esto puede ajustarse con `as const` o type assertions.  
>```typescript
>const obj = { metodo: "GET" } as const; // Ahora, "GET" es literal
>```

>[!danger]- **¿Qué problemas pueden surgir al omitir `as const` en objetos?**  
> Las propiedades del objeto pueden ser tratadas como tipos generales, lo que puede causar errores si se espera un tipo literal.  
>```typescript
>const obj = { metodo: "GET" }; // `metodo` es `string`, no `"GET"`
>function manejar(metodo: "GET" | "POST") {}
>manejar(obj.metodo); // Error
>```
- - - 
#typescript 