# TYPESCRIPT -> MAPPED TYPES
## SUMMARY
> [!summary]
> 
- - - 

## DEFINITION

**Official:**  
> Los **Mapped Types** son una forma de crear nuevos tipos transformando propiedades de un tipo existente, utilizando una sintaxis basada en claves y modificadores.

**Personal:**  
> Un Mapped Type actúa como una "plantilla" que aplica cambios a las propiedades de un tipo existente, ya sea ajustando sus claves o alterando los tipos de sus valores. Es ideal para construir tipos dinámicos o derivados.

---

## THEORY

## B.1- ¿CÓMO FUNCIONAN LOS MAPPED TYPES?

Un Mapped Type recorre las claves de un tipo existente (`T`) y aplica una transformación a sus propiedades. La sintaxis usa un bucle implícito:
```typescript
type NuevoTipo<K extends keyof T> = { [P in K]: Transformación };
```

### EJEMPLO BÁSICO:

```ts
type Usuario = {
  id: number;
  nombre: string;
};

type SoloLectura<T> = { readonly [P in keyof T]: T[P] };

let usuario: SoloLectura<Usuario> = { id: 1, nombre: "Diego" };
// usuario.id = 2; // Error: la propiedad es de solo lectura

```

## B.2- MODIFICADORES EN MAPPED TYPES

1. **`readonly`**: Convierte todas las propiedades en solo lectura.
2. **`?` (opcional):** Hace que todas las propiedades sean opcionales.
3. **`-readonly` o `-?`**: Elimina `readonly` o `?` de las propiedades.

### EJEMPLO CON MODIFICADORES:

```ts
type Usuario = {
  id: number;
  nombre: string;
};

type Parcial<T> = { [P in keyof T]?: T[P] };
type Requerido<T> = { [P in keyof T]-?: T[P] };

let parcial: Parcial<Usuario> = { id: 1 }; // OK: nombre es opcional
let requerido: Requerido<Parcial<Usuario>> = { id: 1, nombre: "Diego" }; // Todo es obligatorio
```

## B.3- USO AVANZADO: HERENCIA Y TRANSFORMACIÓN DE TIPOS

Los Mapped Types permiten extender y transformar tipos complejos, ideal para librerías y API dinámicas.

### EJEMPLO CON HERENCIA:

```ts
type Respuesta<T> = {
  [P in keyof T]: T[P] | null; // Las propiedades pueden ser nulas
};

type Usuario = { id: number; nombre: string; };
type UsuarioRespuesta = Respuesta<Usuario>;

let usuario: UsuarioRespuesta = { id: 1, nombre: null }; // OK
```

# QUESTIONS

>[!tip]- **¿Qué son los Mapped Types en TypeScript?**  
> Son tipos derivados que transforman propiedades de un tipo existente.  
> ```typescript
> type SoloLectura<T> = { readonly [P in keyof T]: T[P] };
> ```

>[!question]- **¿Qué modificadores se pueden usar en Mapped Types?**  
> - `readonly`: Convierte propiedades en solo lectura.  
> - `?`: Convierte propiedades en opcionales.  
> - `-readonly` y `-?`: Remueven las restricciones de solo lectura u opcionalidad.  
> ```typescript
> type Usuario = { id: number; nombre: string };
> type Modificado = { -readonly [P in keyof Usuario]-?: Usuario[P] };
> ```

>[!warning]- **¿Cómo usar Mapped Types con propiedades opcionales?**  
> Usa `?` para transformar todas las propiedades en opcionales.  
> ```typescript
> type Parcial<T> = { [P in keyof T]?: T[P] };
> ```

>[!danger]- **¿Qué precauciones tomar al usar Mapped Types?**  
> Recuerda que un Mapped Type no garantiza validaciones en tiempo de ejecución; solo asegura consistencia en tiempo de desarrollo.  
> ```typescript
> type Usuario = { id: number; nombre: string; };
> type Opcional = { [P in keyof Usuario]?: Usuario[P] };
> let datos: Opcional = {}; // OK en tiempo de desarrollo, pero podría causar problemas si falta información en tiempo de ejecución.
> ```
- - - 
