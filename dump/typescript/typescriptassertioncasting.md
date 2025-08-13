# TYPESCRIPT -> ASSERTION & CASTING
## DEFINICIÓN

**Oficial:**

> Las aserciones de tipo (Type Assertions) en TypeScript son un mecanismo que te permite indicar al compilador el tipo específico de un valor cuando TypeScript no puede deducirlo por sí mismo.

**Personal:**

> Las aserciones de tipo son como decirle a TypeScript "Sé más que tú". Las usas para especificar o "castear" explícitamente el tipo de un valor cuando la inferencia de TypeScript no es suficiente.

---

## TEORÍA

### ¿QUÉ ES UNA ASERCIÓN DE TIPO?

Una aserción de tipo es una forma de decirle a TypeScript que trate un valor como si fuera de un tipo específico. No realiza ninguna validación en tiempo de ejecución; simplemente cambia cómo el compilador interpreta el valor.

#### SINTAXIS DE LAS ASERCIONES DE TIPO:
1. **Usando la palabra clave `as`:**

   ```ts
   let someValue: any = "esto es una cadena";
   let strLength: number = (someValue as string).length;
   ```

2. **Usando la sintaxis de ángulo (`<Tipo>`):**

   ```ts
   let someValue: any = "esto es una cadena";
   let strLength: number = (<string>someValue).length;
   ```

#### PUNTOS CLAVE:
- Las aserciones no realizan comprobaciones en tiempo de ejecución; solo influyen en la verificación de tipos en TypeScript.
- Usa `as` en archivos `.tsx`, ya que los corchetes angulares entran en conflicto con JSX.

#### ASERCIONES AVANZADAS
En casos más estrictos, puedes castear a través de un tipo intermediario como `unknown` o `any` para eludir la verificación de tipos:

```ts
let value: unknown = "123";
let num: number = value as unknown as number;
```

---

### ¿QUÉ ES EL CASTEO?

El casteo en TypeScript es un proceso mediante el cual forzamos la conversión de un valor a un tipo diferente, generalmente usando aserciones. Aunque se usa de manera intercambiable con "aserción", en un contexto técnico, el casteo puede implicar transformaciones más amplias.

#### MÉTODOS DE CASTEO EN TYPESCRIPT:

1. **Con `as`:**
   ```ts
   let input: unknown = "42";
   let numberInput: number = input as number;
   ```

2. **Con Sintaxis de Ángulo (`<Tipo>`):**
   ```ts
   let input: unknown = "42";
   let numberInput: number = <number>input;
   ```

3. **Casteo Seguro con `unknown`:**
   Utiliza `unknown` para garantizar una verificación explícita antes de realizar el casteo:

   ```ts
   function processInput(input: unknown): void {
       if (typeof input === "string") {
           console.log((input as string).toUpperCase());
       }
   }
   ```

---

### CASOS COMUNES

4. **Manipulación del DOM:**

   ```ts
   const element = document.getElementById("myElement") as HTMLDivElement;
   element.style.color = "blue";
   ```

5. **Trabajar con APIs:**

   ```ts
   interface ApiResponse {
       data: any;
   }

   const response: ApiResponse = fetchData();
   const users = response.data as User[];
   ```

6. **Reducir Tipos:**

   ```ts
   function isString(value: unknown): boolean {
       return typeof value === "string";
   }

   if (isString(value)) {
       let length = (value as string).length;
   }
   ```

---

## PREGUNTAS

> [!tip]- **¿Qué son las aserciones de tipo en TypeScript?**
> Son una forma de indicar explícitamente el tipo de un valor cuando TypeScript no puede inferirlo correctamente.
>
> ```ts
> let input: any = "42";
> let num: number = input as number;
> ```

> [!question]- **¿Cuándo deberías usar `as` en lugar de corchetes angulares?**
> En archivos `.tsx`, siempre usa `as`, ya que los corchetes angulares entran en conflicto con la sintaxis JSX.
>
> ```ts
> let element: HTMLElement = document.getElementById("app") as HTMLElement;
> ```

> [!warning]- **¿Cuáles son los riesgos de las aserciones incorrectas?**
> Las aserciones no realizan comprobaciones en tiempo de ejecución, lo que puede generar comportamientos inesperados o errores si el tipo es incorrecto.
>
> ```ts
> let input: any = 42;
> let text: string = input as string; // Puede causar problemas en tiempo de ejecución
> ```

> [!danger]- **¿Por qué es más seguro castear con `unknown`?**
> `unknown` obliga a una verificación explícita antes de reducir un tipo, disminuyendo el riesgo de suposiciones inválidas.
>
> ```ts
> let value: unknown = "hola";
> if (typeof value === "string") {
>     let upper = value.toUpperCase();
> }
> ```
- - - 
#typescript 