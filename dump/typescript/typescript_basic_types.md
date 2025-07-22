# TypeScript: Basic Types

## Definition
**Official:**  
> Los tipos básicos en TypeScript representan los datos primitivos disponibles en JavaScript, incluyendo cadenas, números y booleanos, además de los tipos adicionales como `null`, `undefined` y `any`.

**Personal:**  
> Los tipos básicos son los cimientos de TypeScript. Son como etiquetas para asegurarte de que los valores sean lo que esperas: textos, números, booleanos, entre otros.

---

## Theory

Los **tipos básicos** en TypeScript abarcan desde los primitivos de JavaScript hasta tipos específicos de TypeScript que ofrecen mayor flexibilidad y control en el manejo de datos.

### C.1- Tipos Primitivos
- **`string`:** Representa cadenas de texto.
- **`number`:** Acepta números enteros y decimales.
- **`boolean`:** Representa valores lógicos (`true`/`false`).
- **`null` y `undefined`:** Representan la ausencia de valor, siendo `null` intencional y `undefined` cuando no se inicializa.
- **`any`:** Permite cualquier tipo de dato, pero debe usarse con precaución.

>[!info]- **Ejemplo de tipos básicos**
> ```typescript
> let nombre: string = "Diego";
> let edad: number = 30;
> let activo: boolean = true;
> let indefinido: undefined;
> let nulo: null = null;
> let algo: any = "Puede ser cualquier cosa";
> ```

### C.2- Inferencia de Tipos
TypeScript puede inferir automáticamente el tipo basado en el valor asignado.

>[!info]- **Ejemplo de inferencia**
> ```typescript
> let mensaje = "Hola"; // Infierido como string
> let total = 100;      // Infierido como number
> ```

### C.3- Tipo `unknown`
`unknown` es más seguro que `any` y obliga a comprobar el tipo antes de usarlo.

>[!info]- **Ejemplo de tipo `unknown`**
> ```typescript
> let valor: unknown = "Un valor desconocido";
> if (typeof valor === "string") {
>   console.log("Es una cadena de texto:", valor);
> }
> ```

### C.4- [[typescriptassertioncasting|Casteo]] (`as`)
Permite convertir explícitamente un tipo a otro.

>[!info]- **Ejemplo de casteo**
> ```typescript
> let valor: any = "123";
> let numero: number = valor as number;
> ```

---

## Questions

>[!tip]- **¿Qué tipos básicos existen en TypeScript?**  
> Incluyen:
> - `string`: Representa texto.  
> - `number`: Representa números, enteros o decimales.  
> - `boolean`: Representa valores de verdad (true/false).  
> - `null` y `undefined`: Representan ausencia de valor.  
> - `any`: Acepta cualquier tipo de valor.  
> ```typescript
> let texto: string = "Hola";
> let numero: number = 42;
> let activo: boolean = true;
> let sinValor: undefined;
> let algo: any = "Puede ser cualquier cosa";
> ```

>[!question]- **¿Qué diferencia hay entre `null` y `undefined`?**  
> - `null`: Representa la ausencia intencionada de un valor.  
> - `undefined`: Representa una variable que aún no ha sido inicializada.  
> ```typescript
> let sinDefinir: undefined;
> let vacio: null = null;
> ```

>[!warning]- **¿Qué pasa si no defines un tipo explícito?**  
> TypeScript intentará inferir el tipo basado en el valor asignado.  
> ```typescript
> let numero = 30; // Infierido como number
> ```

>[!danger]- **¿Qué problemas puede causar el tipo `any`?**  
> `any` desactiva el sistema de tipos, permitiendo valores incorrectos. Usarlo indiscriminadamente puede llevar a errores en tiempo de ejecución.  
> ```typescript
> let algo: any = "Texto";
> algo = 42; // Permitido, pero puede ser problemático
> ```
- - - 
#typescript 