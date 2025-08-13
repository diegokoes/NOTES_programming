# TYPESCRIPT: CUSTOM TYPES
## SUMMARY

**Official:**  
> Los **Custom Types** en TypeScript son tipos definidos por los desarrolladores para modelar estructuras de datos y comportamientos específicos, proporcionando flexibilidad y control en los sistemas tipados.

**Personal:**  
> Los tipos personalizados son herramientas que te permiten estructurar datos y lógica de manera más precisa en TypeScript, mejorando la reutilización y seguridad del código.

---

## THEORY

### TIPOS BÁSICOS PERSONALIZADOS

1. **Alias de Tipo (`type`)**:  
   Definen tipos reutilizables para estructuras complejas.  
   >```typescript
   >type Usuario = { nombre: string; edad: number };
   >const usuario: Usuario = { nombre: "Ana", edad: 30 };
   >```

2. **Interfaces (`interface`)**:  
   Útiles para describir objetos y soportan herencia directa.  
   >```typescript
   >interface Persona {
   >  nombre: string;
   >  edad: number;
   >}
   >
   >const persona: Persona = { nombre: "Luis", edad: 25 };
   >```

3. **Uniones e Intersecciones**:  
   - **Uniones** permiten combinar múltiples tipos posibles.  
   - **Intersecciones** exigen cumplir con todos los tipos combinados.  
   >```typescript
   >type Resultado = "aprobado" | "suspenso";
   >type UsuarioAdmin = Persona & { rol: "admin" };
   >```

---

### DIFERENCIAS ENTRE `type` E `interface`

| Característica                | `type`                           | `interface`                      |
|-------------------------------|-----------------------------------|-----------------------------------|
| **Definición de uniones**     | ✅ Sí                            | ❌ No                            |
| **Herencia directa**          | ❌ No                            | ✅ Sí                            |
| **Extensión mediante intersecciones** | ✅ Sí                            | ✅ Sí                            |
| **Tipos primitivos y literales** | ✅ Sí                            | ❌ No                            |

---

### USO AVANZADO DE TIPOS PERSONALIZADOS

1. [**Generics (CustomType)**](typescript_generics_customtype.md):  
   Crear tipos que aceptan parámetros para mayor reutilización.

2. [**Keyof Type Operator (CustomType)**](typescript_keyof_operator_customtype.md):  
   Obtener las claves de un tipo como una unión.

3. [**Typeof Type Operator (CustomType)**](typescript_typeof_operator_customtype.md):  
   Crear tipos basados en valores existentes.

4. [**Indexed Access Types (CustomType)**](typescript_indexed_access_customtype.md):  
   Extraer tipos específicos de una propiedad de un objeto.

5. [**Conditional Types (CustomType)**](typescript_conditional_types_customtype.md):  
   Tipos definidos en base a condiciones lógicas.

6. [**Mapped Types (CustomType)**](typescript_mapped_types_customtype.md):  
   Transformar las propiedades de un tipo.

7. [**Template Literal Types (CustomType)**](typescript_template_literal_customtype.md):  
   Crear tipos dinámicos basados en cadenas literales.


Cada uno de estos temas se encuentra explicado con detalle en su respectiva nota.

---

## QUESTIONS

>[!tip]- **¿Qué son los tipos personalizados en TypeScript?**  
> Son definiciones específicas creadas por el desarrollador para modelar datos y comportamientos.  
>```typescript
>type Usuario = { nombre: string; edad: number };
>```

>[!question]- **¿Cuándo usar `type` y cuándo usar `interface`?**  
> Usa `interface` para objetos y herencia; `type` para uniones, literales o combinaciones avanzadas.  
>```typescript
>type Rol = "admin" | "user";
>interface Usuario { nombre: string; edad: number; rol: Rol; }
>```

>[!warning]- **¿Qué pasa si abusas de `any` en tipos personalizados?**  
> Pierdes la ventaja del sistema de tipos, creando estructuras más propensas a errores.  
>```typescript
>type Inseguro = any; // Evítalo siempre que puedas
>```

>[!danger]- **¿Cómo puedes usar tipos personalizados avanzados?**  
> Aprovecha operadores como `keyof`, `typeof`, o mapeos para manipular tipos existentes.  
>```typescript
>type PropiedadesUsuario = keyof Usuario; // "nombre" | "edad"
>```
- - - 
#typescript 