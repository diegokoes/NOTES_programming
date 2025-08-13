# TYPESCRIPT -> UTILITY TYPES

## SUMMARY
> [!summary]-
> 
- - - 

## DEFINITION

**Official:**  
> Los **Utility Types** son tipos predefinidos en TypeScript que permiten realizar transformaciones comunes en tipos existentes, como hacerlos opcionales, de solo lectura, o seleccionar o excluir ciertas propiedades.

**Personal:**  
> Los Utility Types son herramientas prácticas que te ahorran tiempo al manipular tipos, como transformar estructuras complejas de manera eficiente para ajustarse a tus necesidades.

---

## THEORY

## B.1- TIPOS PRINCIPALES DE UTILITY TYPES

### **1. `Partial<T>`**
Convierte todas las propiedades de un tipo en opcionales.

```typescript
type Usuario = {
  id: number;
  nombre: string;
};

type UsuarioParcial = Partial<Usuario>;

let usuario: UsuarioParcial = { nombre: "Diego" }; // OK: `id` es opcional
```

### **2. `Required<T>`**

Convierte todas las propiedades de un tipo en obligatorias.

```ts
type Usuario = {
  id?: number;
  nombre?: string;
};

type UsuarioRequerido = Required<Usuario>;

let usuario: UsuarioRequerido = { id: 1, nombre: "Diego" }; // Todo es obligatorio
```

### **3. `Readonly<T>`**

Convierte todas las propiedades de un tipo en solo lectura.

```ts
type Usuario = {
  id: number;
  nombre: string;
};

type UsuarioSoloLectura = Readonly<Usuario>;

let usuario: UsuarioSoloLectura = { id: 1, nombre: "Diego" };
// usuario.id = 2; // Error: la propiedad es de solo lectura
```

### **4. `Pick<T, K>`**

Selecciona un subconjunto de propiedades de un tipo.

```ts
type Usuario = {
  id: number;
  nombre: string;
  email: string;
};

type UsuarioBasico = Pick<Usuario, "id" | "nombre">;

let usuario: UsuarioBasico = { id: 1, nombre: "Diego" }; // Solo incluye `id` y `nombre`
```

### **5. `Omit<T, K>`**

Excluye un subconjunto de propiedades de un tipo.

```ts
type Usuario = {
  id: number;
  nombre: string;
  email: string;
};

type UsuarioSinEmail = Omit<Usuario, "email">;

let usuario: UsuarioSinEmail = { id: 1, nombre: "Diego" }; // Excluye `email`
```
- - - 
#typescript 