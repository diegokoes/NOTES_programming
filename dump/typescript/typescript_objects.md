# TypeScript -> Objects

## Summary
> [!summary]-
> 
- - - 

## Definition

**Official:**  
> En TypeScript, los objetos representan estructuras de datos que agrupan propiedades y métodos. Pueden ser definidos mediante interfaces, tipos alias o de forma anónima. Proporcionan flexibilidad y control mediante modificadores como `readonly` y propiedades opcionales.

**Personal:**  
> Los objetos en TypeScript son herramientas fundamentales para organizar datos y comportamientos. Te permiten definir estructuras claras, controlar cómo se accede y modifica cada propiedad, y garantizar consistencia en tu código.

---

## Theory

## B.1- Definición de Objetos

Los objetos en TypeScript pueden definirse de varias maneras:

1. **Anónimos**:
>[!info]- **Ejemplo de Objeto Anónimo**
>```typescript
>function saludar(persona: { nombre: string; edad: number }): string {
>  return `Hola, ${persona.nombre}`;
>}
>
>saludar({ nombre: "Diego", edad: 30 }); // "Hola, Diego"
>```

2. **Mediante Interfaces**:
>[!info]- **Ejemplo con Interfaces**
>```typescript
>interface Persona {
>  nombre: string;
>  edad: number;
>}
>
>function saludar(persona: Persona): string {
>  return `Hola, ${persona.nombre}`;
>}
>```

3. **Mediante Type Alias**:
>[!info]- **Ejemplo con Type Alias**
>```typescript
>type Persona = {
>  nombre: string;
>  edad: number;
>};
>
>function saludar(persona: Persona): string {
>  return `Hola, ${persona.nombre}`;
>}
>```

---

## B.2- Propiedades Opcionales

Las propiedades de un objeto pueden ser opcionales usando `?`.

>[!info]- **Propiedades Opcionales**
>```typescript
>interface OpcionesDibujo {
>  forma: string;
>  x?: number;
>  y?: number;
>}
>
>function dibujar(opciones: OpcionesDibujo): void {
>  const x = opciones.x ?? 0; // Predeterminado a 0
>  const y = opciones.y ?? 0;
>  console.log(`Dibujando en (${x}, ${y})`);
>}
>
>dibujar({ forma: "círculo" });         // (0, 0)
>dibujar({ forma: "círculo", x: 10 }); // (10, 0)
>```

---

## B.3- Modificador `readonly`

Las propiedades marcadas con `readonly` no pueden ser reasignadas después de su inicialización.

>[!info]- **Propiedades de Solo Lectura**
>```typescript
>interface Casa {
>  readonly direccion: string;
>  propietario: { nombre: string; edad: number };
>}
>
>const casa: Casa = {
>  direccion: "Calle Falsa 123",
>  propietario: { nombre: "Diego", edad: 30 },
>};
>
>casa.propietario.edad += 1; // Permitido
>casa.direccion = "Otra dirección"; // Error
>```

---

## B.4- Firmas de Índice (Index Signatures)

Permiten describir objetos con claves dinámicas.

>[!info]- **Firmas de Índice**
>```typescript
>interface Diccionario {
>  [clave: string]: string;
>}
>
>const diccionario: Diccionario = {
>  hola: "hello",
>  mundo: "world",
>};
>
>console.log(diccionario["hola"]); // "hello"
>```

---

## B.5- Exceso de Propiedades

TypeScript valida que los objetos no tengan propiedades no definidas en su tipo.

>[!info]- **Validación de Exceso de Propiedades**
>```typescript
>interface Configuracion {
>  color?: string;
>  ancho?: number;
>}
>
>function crearCuadrado(config: Configuracion): void {
>  console.log(`Creando cuadrado de color ${config.color}`);
>}
>
>crearCuadrado({ colour: "rojo" }); // Error: `colour` no está definido
>crearCuadrado({ color: "rojo" });  // OK
>```

Soluciones:  
- **Type Assertions**:
>```typescript
>crearCuadrado({ colour: "rojo" } as Configuracion); // Ignora la validación
>```

- **Índice Genérico**:
>```typescript
>interface Configuracion {
>  color?: string;
>  ancho?: number;
>  [propiedad: string]: unknown;
>}
>```

---

## B.6- Extensiones de Tipos

Los tipos pueden extenderse para evitar repetición de código.

>[!info]- **Extensión con Interfaces**
>```typescript
>interface Direccion {
>  calle: string;
>  ciudad: string;
>}
>
>interface DireccionCompleta extends Direccion {
>  codigoPostal: string;
>}
>
>const miDireccion: DireccionCompleta = {
>  calle: "Calle 1",
>  ciudad: "Ciudad 1",
>  codigoPostal: "12345",
>};
>```

---

## B.7- Tipos Genéricos

Permiten definir objetos que aceptan tipos dinámicos.

>[!info]- **Tipos Genéricos**
>```typescript
>interface Caja<T> {
>  contenido: T;
>}
>
>const cajaDeTexto: Caja<string> = { contenido: "Hola" };
>const cajaDeNumero: Caja<number> = { contenido: 42 };
>```

---

## Questions

>[!tip]- **¿Cómo se definen objetos en TypeScript?**  
> Pueden definirse mediante objetos anónimos, interfaces o type alias.  
>```typescript
>type Persona = { nombre: string; edad: number };
>```

>[!question]- **¿Qué son las propiedades opcionales en TypeScript?**  
> Son propiedades que pueden no estar presentes en un objeto. Se definen con `?`.  
>```typescript
>interface Config { ancho?: number; }
>```

>[!warning]- **¿Qué es una firma de índice y para qué sirve?**  
> Permite definir objetos con claves dinámicas.  
>```typescript
>interface Diccionario { [clave: string]: string; }
>```

>[!danger]- **¿Cómo se manejan los excesos de propiedades?**  
> Se usan type assertions o firmas de índice.  
>```typescript
>interface Config { color?: string; ancho?: number; [propiedad: string]: unknown; }
>```
- - - 
#typescript 