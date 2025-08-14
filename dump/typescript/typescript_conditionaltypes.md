# TYPESCRIPT -> CONDITIONAL TYPES
## SUMMARY
> [!summary]-
> 
- - - 
## DEFINITION

**Official:**  
> Los **Conditional Types** son una característica avanzada de TypeScript que permite definir tipos basados en una condición lógica, utilizando la sintaxis `T extends U ? X : Y`.

**Personal:**  
> Un Conditional Type evalúa una condición entre dos tipos (`T` y `U`) y selecciona un tipo resultante (`X` o `Y`). Es como un operador ternario, pero aplicado a tipos, y es extremadamente útil para construir tipos dinámicos y flexibles.

---

## THEORY

## B.1- ¿CÓMO FUNCIONAN LOS CONDITIONAL TYPES?

Un Conditional Type utiliza `extends` para verificar si un tipo cumple una condición. La sintaxis básica es:

```typescript
T extends U ? X : Y
```

- Si `T` extiende (o es compatible con) `U`, devuelve `X`.
- Si no, devuelve `Y`.

### EJEMPLO BÁSICO:
```ts
type TipoCondicional<T> = T extends string ? "Es texto" : "No es texto";

type Resultado1 = TipoCondicional<string>;  // "Es texto"
type Resultado2 = TipoCondicional<number>;  // "No es texto"
```

## B.2- USO COMÚN DE CONDITIONAL TYPES

1. **Filtrado de Tipos**: Eliminar tipos innecesarios.
2. **Mapeo Condicional**: Transformar propiedades en un tipo.
3. **Inferencia de Tipos**: Extraer tipos específicos.

### EJEMPLO CON INFERENCIA:

```ts
type ExtraerArray<T> = T extends (infer U)[] ? U : T;

type Tipo1 = ExtraerArray<number[]>; // number
type Tipo2 = ExtraerArray<string[]>; // string
type Tipo3 = ExtraerArray<boolean>;  // boolean
```

### EJEMPLO CON MAPEO CONDICIONAL:

```ts
type SoloStrings<T> = {
  [K in keyof T]: T[K] extends string ? K : never;
}[keyof T];

type MiObjeto = { nombre: string; edad: number; activo: boolean };
type PropiedadesStrings = SoloStrings<MiObjeto>; // "nombre"
```

## QUESTIONS

> [!tip]- **¿Qué es un Conditional Type en TypeScript?**  
> Es un tipo que selecciona entre dos opciones basándose en una condición lógica.
> ```ts
>type TipoCondicional<T> = T extends string ? "Es texto" : "No es texto";
>```

>[!question]- **¿Cómo se usan los Conditional Types para filtrar tipos?**  
Verifica si un tipo cumple una condición y devuelve el tipo correspondiente.
>```ts
>type SoloTexto<T> = T extends string ? T : never;
>type Texto = SoloTexto<"Hola" | 42 | true>; // "Hola"
>```

>[!warning]- **¿Qué es `infer` y cómo se usa en Conditional Types?**  
>`infer` extrae un tipo dentro de una condición lógica.
>```ts
>type Extraer<T> = T extends (infer U)[] ? U : T;
>type Tipo = Extraer<number[]>; // number
>```

>[!danger]- **¿Qué problemas puede causar un Conditional Type mal diseñado?**  
>Puede hacer el código más difícil de leer y depurar si se combina con estructuras complejas. 
>Además, puede generar resultados inesperados si no se tienen en cuenta todos los casos posibles.
>```ts
>type CondicionalIncompleto<T> = T extends string ? "Texto"; // Falta el caso contrario
>```
- - - 
