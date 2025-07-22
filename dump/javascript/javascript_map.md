# JavaScript -> Map
### Definition
**Official:**
> El método .map() crea un nuevo array a partir de aplicar una función a cada elemento del array original. Este método no modifica el array original y es una forma funcional de transformar datos en JavaScript.

**Personal:**
>.map() es como una "fábrica" para arrays. Toma cada elemento del array original, lo pasa por una función (donde decides qué hacer con él) y devuelve un nuevo array con los resultados. Es súper útil para transformar datos.

- - - 

#### ¿Cómo funciona .map()?

  - > [!info]- Click to reveal the answer
.map() itera por cada elemento del array original y ejecuta una función que defines, devolviendo un nuevo array con los resultados de esa función.

#### ¿Modifica .map() el array original?

  - > [!info]- Click to reveal the answer
No, .map() no cambia el array original. Siempre crea uno nuevo.

#### Qué parámetros admite
  - > [!info]- Click to reveal the answer
array.map((element, index, array) => {
 // lógica para transformar cada elemento
});
- - - 
#javascript 