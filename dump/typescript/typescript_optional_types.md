# TYPESCRIPT -> OPTIONAL TYPES
## SUMMARY
> [!summary]
> 
- - - 

## THEORY

## B.1- PROPIEDADES OPCIONALES

Las propiedades opcionales se definen añadiendo `?` después del nombre de la propiedad.

>[!info]- **Ejemplo de Propiedades Opcionales**  
>```typescript
>interface Usuario {
>  id: number;
>  nombre?: string; // Opcional
>}
>
>const usuario1: Usuario = { id: 1 };          // Válido
>const usuario2: Usuario = { id: 2, nombre: "Ana" }; // También válido
>```

## B.2- PARÁMETROS OPCIONALES EN FUNCIONES

Los parámetros opcionales se definen con `?`, y deben estar al final de la lista de parámetros.

>[!info]- **Ejemplo de Parámetros Opcionales**  
>```typescript
>function saludar(nombre?: string): string {
>  return nombre ? `Hola, ${nombre}` : "Hola, invitado";
>}
>
>console.log(saludar());           // "Hola, invitado"
>console.log(saludar("Diego"));    // "Hola, Diego"
>```

## B.3- VALORES POR DEFECTO PARA OPCIONALES

Los parámetros opcionales pueden tener valores por defecto, lo cual mejora la legibilidad y manejo de casos.

>[!info]- **Ejemplo de Valores por Defecto**  
>```typescript
>function configurar(opciones?: { modo: string } = { modo: "predeterminado" }) {
>  console.log(opciones.modo);
>}
>
>configurar();                        // "predeterminado"
>configurar({ modo: "avanzado" });    // "avanzado"
>```

---

## QUESTIONS

>[!tip]- **¿Qué son los tipos opcionales en TypeScript?**  
> Son propiedades o parámetros que pueden omitirse sin causar errores de tipo.  
>```typescript
>interface Usuario { id: number; nombre?: string; }
>```

>[!warning]- **¿Cómo defines una propiedad opcional?**  
> Usa el símbolo `?` después del nombre de la propiedad.  
>```typescript
>interface Config { tema?: string; }
>```

>[!warning]- **¿Qué sucede si intentas acceder a una propiedad opcional que no existe?**  
> TypeScript permitirá el acceso, pero podría devolver `undefined`. Usa un operador de coalescencia nula (`??`) o validación para manejarlo.  
>```typescript
>interface Config { tema?: string; }
>const config: Config = {};
>console.log(config.tema ?? "No definido"); // "No definido"
>```

>[!danger]- **¿Qué errores pueden ocurrir con parámetros opcionales?**  
> Si no manejas correctamente un parámetro opcional, puedes encontrarte con `undefined` en tiempo de ejecución, causando errores inesperados.  
>```typescript
>function sumar(a: number, b?: number): number {
>  return a + (b ?? 0);
>}
>console.log(sumar(5)); // 5
>console.log(sumar(5, 3)); // 8
>```

---
