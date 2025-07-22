# TypeScript -> Functions
## Definition

**Oficial:**
> En TypeScript, las funciones son bloques de código reutilizables que realizan una tarea específica. Las funciones pueden tener parámetros con tipos explícitos, y se puede definir el tipo de su valor de retorno.

**Personal:**
> Una función es como una "caja mágica" que toma entradas (parámetros), realiza operaciones y devuelve un resultado. En TypeScript, puedes definir tipos para las entradas y salidas, haciéndolas más seguras.

---

## Theory

## B.1- Funciones en TypeScript

Las funciones pueden ser declarativas, expresivas o incluso anidadas. Además, permiten una gran variedad de estilos dependiendo del contexto y los requisitos del programa.

### **Tipos de Funciones**

1. **Funciones Declarativas:**
   Son la forma clásica de definir una función. Se declaran usando la palabra clave `function`.
   ```ts
   function sumar(a: number, b: number): number {
       return a + b;
   }
   ```

2. **Funciones como Expresiones:**
   Estas funciones se asignan a una variable. Aunque son similares a las declarativas, no tienen un nombre propio.
   ```ts
   const restar = function (a: number, b: number): number {
       return a - b;
   };
   ```

3. **Funciones Flecha:**
   Usan una sintaxis más compacta y no enlazan su propio valor de `this`. Son ideales para funciones cortas.
   ```ts
   const multiplicar = (a: number, b: number): number => a * b;
   ```

4. **Funciones Anidadas:**
   Puedes definir funciones dentro de otras funciones para encapsular lógica específica.
   ```ts
   function operacion(a: number, b: number): number {
       function suma(x: number, y: number): number {
           return x + y;
       }
       return suma(a, b);
   }
   ```

5. **Funciones como Callbacks:**
   Una función puede pasarse como argumento a otra función.
   ```ts
   function procesarCallback(a: number, b: number, callback: (resultado: number) => void): void {
       const resultado = a + b;
       callback(resultado);
   }

   procesarCallback(5, 10, (res) => console.log(res)); // 15
   ```

### **Parámetros Opcionales y Predeterminados**

1. **Parámetros Opcionales:** Se definen usando `?` y pueden omitirse al llamar a la función.
   ```ts
   function saludar(nombre?: string): string {
       return nombre ? `Hola, ${nombre}` : "Hola, invitado";
   }
   ```

2. **Parámetros Predeterminados:** Proveen un valor por defecto si no se pasa uno.
   ```ts
   function multiplicar(a: number, b: number = 1): number {
       return a * b;
   }
   ```

### **Tipos para el Retorno**
Puedes especificar el tipo del valor que devuelve una función:
```ts
function dividir(a: number, b: number): number {
    return a / b;
}
```

Si no devuelve nada, usa el tipo `void`:
```ts
function logMensaje(mensaje: string): void {
    console.log(mensaje);
}
```

### **Funciones con Tipos Generics**
Los tipos genéricos permiten que las funciones trabajen con diferentes tipos, manteniendo la tipificación:
```ts
function identidad<T>(valor: T): T {
    return valor;
}
```

### **Funciones Lambda en Declaraciones Complejas**
Puedes combinar funciones flecha dentro de declaraciones de funciones tradicionales para mayor flexibilidad.
```ts
function operacionesAvanzadas(a: number, b: number): number {
    const sumar = (x: number, y: number): number => x + y;
    const multiplicar = (x: number, y: number): number => x * y;
    return sumar(a, b) + multiplicar(a, b);
}
```

---

##C- Questions

> [!tip]- **¿Cómo se define una función en TypeScript?**
> Usando la palabra clave `function`, seguida del nombre de la función, los parámetros con sus tipos, y el tipo del valor de retorno.
> ```ts
> function saludar(nombre: string): string {
>     return `Hola, ${nombre}`;
> }
> ```

> [!tip]- **¿Qué ventajas tiene usar funciones flecha?**
> Las funciones flecha son más compactas y no enlazan su propio valor de `this`, haciéndolas ideales para callbacks o funciones cortas.
> ```ts
> const sumar = (a: number, b: number): number => a + b;
> ```

> [!question]- **¿Qué son los parámetros opcionales en una función?**
> Son parámetros que pueden omitirse al llamar a la función. Se definen con `?` después del nombre del parámetro.
> ```ts
> function sumarOpcional(a: number, b?: number): number {
>     return b ? a + b : a;
> }
> ```

> [!question]- **¿Cuándo usar parámetros predeterminados en lugar de opcionales?**
> Usa predeterminados cuando quieras garantizar un valor inicial en caso de que no se pase el argumento.
> ```ts
> function multiplicar(a: number, b: number = 2): number {
>     return a * b;
> }
> ```

> [!warning]- **¿Qué sucede si no defines un tipo de retorno explícito?**
> TypeScript intentará inferir el tipo, pero es una buena práctica definirlo para mayor claridad y seguridad.
> ```ts
> function inferido(a: number, b: number) {
>     return a + b; // Infierido como number
> }
> ```

> [!warning]- **¿Qué problemas pueden surgir con funciones sin tipo?**
> Pueden llevar a errores si los parámetros o el retorno no son lo que esperas.
> ```ts
> function procesar(a, b) {
>     return a + b; // Puede causar errores en tipos no numéricos
> }
> ```

> [!danger]- **¿Por qué usar generics en funciones?**
> Los genéricos permiten que las funciones sean reutilizables y trabajen con diferentes tipos mientras mantienen la seguridad de tipos.
> ```ts
> function wrapInArray<T>(valor: T): T[] {
>     return [valor];
> }
> ```

> [!danger]- **¿Qué beneficios tiene usar funciones como callbacks?**
> Permiten manejar eventos o procesos asincrónicos de forma más clara y modular.
> ```ts
> function ejecutarProceso(callback: () => void): void {
>     console.log("Iniciando proceso...");
>     callback();
> }
>
> ejecutarProceso(() => console.log("Proceso completado"));
> ```
- - - 
[[javascript_functions|Funciones JavaScript]] #typescript 