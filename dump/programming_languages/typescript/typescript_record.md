# TYPESCRIPT -> RECORD
## DEFINITION

**Official:**  
> El tipo `Record<K, T>` en TypeScript crea un objeto cuyas claves son un conjunto de valores `K` y cuyos valores son del tipo `T`.

**Personal:**  
> `Record` es como un "mapa" de claves y valores, donde defines qué claves están permitidas y qué tipo tendrán sus valores. Es útil para estructuras de datos consistentes y predecibles.

---

## THEORY

## B.1- SINTAXIS Y USO BÁSICO

El tipo `Record` utiliza dos parámetros genéricos:  
- `K`: Representa las claves. Deben ser un conjunto de valores literales (strings, números, etc.).  
- `T`: Representa el tipo de los valores asociados a las claves.

>[!info]- **Sintaxis de `Record`**  
> ```typescript
> type Record<K extends keyof any, T> = {
>   [P in K]: T;
> };
> ```

### EJEMPLO BÁSICO:
>[!info]- **Mapear roles a valores booleanos**  
> ```typescript
> type Roles = "admin" | "editor" | "viewer";
> 
> type Permisos = Record<Roles, boolean>;
> 
> const permisos: Permisos = {
>   admin: true,
>   editor: false,
>   viewer: true,
> };
> ```

---

## B.2- CASOS DE USO COMUNES

### **1. DEFINIR CONFIGURACIONES**
>[!info]- **Ejemplo de configuración para entornos**  
> ```typescript
> type Configuracion = Record<"desarrollo" | "produccion", string>;
> 
> const urls: Configuracion = {
>   desarrollo: "http://localhost:3000",
>   produccion: "https://miapp.com",
> };
> ```

### **2. MAPEAR IDS A DATOS**
>[!info]- **Ejemplo de mapeo de IDs a usuarios**  
> ```typescript
> type Usuario = { id: number; nombre: string };
> 
> type Usuarios = Record<number, Usuario>;
> 
> const usuarios: Usuarios = {
>   1: { id: 1, nombre: "Diego" },
>   2: { id: 2, nombre: "Ana" },
> };
> ```

### **3. MAPEAR ENUMS A VALORES**
>[!info]- **Ejemplo de mapeo de enums a valores booleanos**  
> ```typescript
> enum Estados {
>   Activo = "activo",
>   Inactivo = "inactivo",
> }
> 
> type EstadoPermisos = Record<Estados, boolean>;
> 
> const permisosEstados: EstadoPermisos = {
>   [Estados.Activo]: true,
>   [Estados.Inactivo]: false,
> };
> ```

---

## QUESTIONS

>[!tip]- **¿Qué es el tipo `Record` en TypeScript?**  
> Es un tipo genérico que mapea un conjunto de claves a un tipo de valores.  
> ```typescript
> type Roles = "admin" | "editor";
> type Permisos = Record<Roles, boolean>;
> ```

>[!question]- **¿Qué tipos pueden ser usados como claves (`K`) en un `Record`?**  
> Las claves pueden ser strings, números o símbolos.  
> ```typescript
> type Configuracion = Record<"desarrollo" | "produccion", string>;
> ```

>[!warning]- **¿Qué pasa si defines una clave que no está en el conjunto permitido?**  
> TypeScript mostrará un error, ya que las claves deben coincidir exactamente con las definidas en `K`.  
> ```typescript
> type Roles = "admin" | "editor";
> const permisos: Record<Roles, boolean> = {
>   admin: true,
>   editor: true,
>   viewer: false, // Error: "viewer" no está definido en `Roles`.
> };
> ```

>[!danger]- **¿Qué ventajas y limitaciones tiene `Record` frente a un objeto tradicional?**  
> Ventajas:  
> - Asegura que las claves sean las definidas y los valores sean del tipo correcto.  
> Limitaciones:  
> - No permite claves dinámicas fuera del conjunto especificado.  
> ```typescript
> type Permisos = Record<"admin" | "editor", boolean>;
> const permisos: Permisos = { admin: true, editor: false }; // Correcto
> const permisosDinamico = { admin: true, editor: false, viewer: true }; // Error
> ```
- - - 
