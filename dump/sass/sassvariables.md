# SASS -> VARIABLES
## DEFINITION
**Official:**
> Las **variables en Sass** son valores reutilizables definidos con un nombre específico, que permiten gestionar colores, tamaños, fuentes y otros valores de diseño en un proyecto SCSS.

**Personal:**
>Las variables de Sass son como "cajas" donde guardas valores que puedes reutilizar en todo tu proyecto. Son útiles para cambiar un valor en un solo lugar y que se refleje en todo el diseño.
- - - 
## QUESTIONS
>[!tip]- **¿Cómo se define una variable en Sass?**  
>Se utiliza el símbolo `$` seguido del nombre de la variable y su valor.```
>$color-primario: #3498db;```

>[!question]- **¿Cómo se usan las variables en SCSS?**  
>Una vez definida, puedes usar la variable en cualquier lugar de tu archivo SCSS.  
>```body {  background-color: $color-primario; }```

>[!warning]- **¿Cómo podrías manejar valores por defecto en variables SCSS?**  
>Usando el modificador `!default`, puedes definir un valor solo si la variable aún no tiene uno.
>`$margen: 16px !default;`

>[!warning]- **¿Qué ocurre si declaras una variable con el mismo nombre en diferentes bloques?**
>El bloque más cercano (local) tomará prioridad sobre variables globales. Puedes usar `!global` si necesitas modificar la variable global desde un bloque.

>[!danger]- **¿Qué diferencias hay entre variables de Sass y variables CSS?**
>- Las variables Sass son compiladas y no existen en el archivo CSS final.
>- Las variables CSS (`--nombre-variable`) permanecen en el CSS y pueden cambiar según el contexto (e.g., por tema o elemento).

## USE

###  **1. USO DE VARIABLES CON OPERACIONES**
```sass
$base: 16px; 
$margen: $base * 2; ◘
							.container { 
										margin: $margen; // 32px 
											}
```

###  CONFIGURACIÓN DE MÓDULOS CON VARIABLES POR DEFECTO

Las variables definidas con `!default` en un módulo pueden ser configuradas al cargarlo con `@use`.

**`_library.scss`**
```scss
$color-primario: #3498db !default;
$espaciado: 16px !default;

body {
  background-color: $color-primario;
  margin: $espaciado;
}
```
**`styles.scss`**
```scss
@use 'library' with (
  $color-primario: #e74c3c,
  $espaciado: 20px
);
```
###  SHADOWING DE VARIABLES
Es posible tener variables locales que "sombran" variables globales con el mismo nombre. Sin embargo, estas no las reemplazan fuera de su bloque.
```scss
$variable: global value;

.content {
  $variable: local value;
  color: $variable; // Usará "local value"
}

.sidebar {
  color: $variable; // Usará "global value"
}
```

###  VARIABLES EN CONTROL DE FLUJO
Dentro de estructuras condicionales (`@if`, `@for`, `@each`), las variables asignadas modifican el valor existente en el ámbito actual. Si no están definidas previamente, deben ser inicializadas como `null`.

```scss
$dark-theme: true !default;
$primary-color: #f8bbd0 !default;

@if $dark-theme {
  $primary-color: darken($primary-color, 60%);
}

.button {
  background-color: $primary-color;
}
```

## C.2- **AVANZADO: VERIFICAR EXISTENCIA DE VARIABLES**

Sass proporciona funciones del módulo `meta` para comprobar si una variable está definida en el contexto actual o en el global. Estas funciones son especialmente útiles para evitar errores en proyectos complejos o en librerías reutilizables.

###  **FUNCIONES CLAVE**

1. **`meta.variable-exists(name)`**
    
    - Comprueba si una variable con el nombre especificado existe en el ámbito actual (local o global).
    - Retorna `true` si la variable está definida, de lo contrario, retorna `false`.
    
```scss
@use "sass:meta";  
$color: #3498db;  

@if meta.variable-exists(color) {   
	.alert {     
			color: $color;   
			} 
	}
```
    
2. **`meta.global-variable-exists(name)`**
    
    - Verifica si una variable está definida en el ámbito global.
    - Retorna `true` si la variable global existe.
    
```scss
@use "sass:meta";  
$global-variable: "I'm global!";  

@if meta.global-variable-exists(global-variable) {   
			.global-check {     
							content: $global-variable;  
						  } 
}
```
    
###  **CÓMO USAR LA VERIFICACIÓN DE VARIABLES**

####  **1. VALIDAR VARIABLES OPCIONALES**

Cuando desarrollas librerías o temas, puedes verificar si una variable está definida antes de usarla para evitar errores.

```scss
@use "sass:meta";  
$primary-color: #3498db !default;  

@mixin button($color) {   
	@if not meta.variable-exists(primary-color) {     
					@error "La variable $primary-color no está definida.";   
							        		}    
	background-color: $color or $primary-color; 
}
```

####  **2. DEBUGGING EN SASS**

Sass incluye directivas útiles para depurar problemas relacionados con variables:

#### - **`@debug`: IMPRIME INFORMACIÓN**

Usa `@debug` para mostrar información útil durante la compilación.


```scss
@debug $primary-color; // Muestra el valor de $primary-color`
```
####  **`@warn`: ADVERTENCIAS**

Emite un mensaje de advertencia cuando algo no cumple una condición, pero continúa con la compilación.

scss

Copy code

`@warn "El valor de $padding es muy grande." if $padding > 100px;`

####  **`@error`: INTERRUMPE LA COMPILACIÓN**

Emite un mensaje de error y detiene la compilación.

scss

Copy code

`@error "El valor de $padding debe ser menor que 50px." if $padding > 50px;`

---

###  **EJEMPLO COMPLETO: RESTRINGIR VALORES DE PADDING**

Supongamos que quieres restringir el uso de `padding` para que solo acepte valores relacionados con `top` y emita un error si se intenta usar `left`.


```scss
@mixin validate-padding($direction, $value) {   
	@if $direction == top {     
		padding-top: $value;   
			} @else {     
				@error "Solo se permite 'top' como dirección de padding. Se                             recibió '#{$direction}'.";   
				} 
}  
.container {   
	@include validate-padding(top, 20px); // Funciona   
	// @include validate-padding(left, 20px); // Error 
}
```

###  **CÓMO LIMITAR O CONTROLAR OPCIONES**

Puedes usar listas para restringir valores aceptados y evitar errores en tus mixins o configuraciones.

####  **EJEMPLO: LISTA DE VALORES PERMITIDOS**

```scss
$allowed-directions: top, right, bottom, left;  
@mixin validate-direction($direction) {   
	@if not index($allowed-directions, $direction) {    
		 @error "La dirección '#{$direction}' no es válida. Use: #{join($allowed-directions, ', ')}.";   
	 } 
}  
.container {   
	@include validate-direction(top);    // Funciona   
	@include validate-direction(center); // Error 
}
```
---

###  **USO REAL: CONDICIONALES CON VARIABLES**

####  **1. COMPROBAR SI UNA VARIABLE ESTÁ INICIALIZADA**

Esto es útil para evitar sobrescribir configuraciones globales:


```scss
@use "sass:meta";  
@mixin initialize-variable($name, $value) {   
	@if not meta.variable-exists($name) {     
			$name: $value !global;   
		} 
}  
@include initialize-variable(primary-color, #3498db);
```

####  **2. COMBINAR `@error` Y VALIDACIÓN**

Crea mixins robustos para detectar configuraciones incorrectas:

```scss
@mixin validate-and-set($variable, $value) {   
		@if meta.variable-exists($variable) {     
			@warn "La variable #{$variable} ya está definida. Su valor será sobrescrito.";   
				}    
		$variable: $value !global; 
}  
@include validate-and-set(primary-color, #e74c3c);
```

!TODO LISTAS Y MAPAS 

- - - 
#sass 