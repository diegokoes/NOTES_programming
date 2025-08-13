# HTML

## BASICS

1. [Basic Structure](html_structure.md)
2. [Head Elements](html_head.md)
3. [Block Elements](html_block.md)
4. [Inline Elements](html_inline.md)


## FORMS

5. [Form Basics](html_form_basics.md)
6. [Input Fields](html_input_fields.md)
7. [Form Validation](html_form_validation.md)
8. [Labels and Buttons](html_labels_buttons.md)
9. [Advanced Form Features](html_form_advanced.md)

## MULTIMEDIA

10. [Images](html_images.md)
11. [Audio](html_audio.md)
12. [Video](html_video.md)
13. [Responsive Images with `<picture>`](html_picture.md)

## TABLES

14. [Creating Tables](html_tables.md)
15. [Table Accessibility](html_table_accessibility.md)

## ACCESSIBILITY

16. [ARIA Roles and Attributes](html_aria.md)
17. [Accessible Forms](html_accessible_forms.md)
18. [Details and Summary](html_details_summary.md)

## ADVANCED FEATURES

19. [Custom Data Attributes (`data-*`)](html_data_attributes.md)
20. [Graphics with Canvas](html_canvas.md)
21. [Graphics with SVG](html_svg.md)

## LISTS

22. [Unordered Lists](html_unordered_lists.md)
23. [Ordered Lists](html_ordered_lists.md)
24. [Definition Lists](html_definition_lists.md)

## ANCHORS AND LINKS

25. [Anchor Links](html_anchors.md)
26. [Internal and External Links](html_links.md)


# ETIQUETAS DE TEXTO

```html
<div contenteditable>
	<p> lore ipsum...</p>
</div>
```



## **`<em>` VS `<i>`**

- **`<em>`**: Representa énfasis en el texto y tiene significado semántico. Los lectores de pantalla aplican énfasis al leerlo.
- **`<i>`**: Coloca el texto en cursiva pero no tiene significado semántico. Se usa para términos técnicos, frases en otro idioma o títulos de obras.

```HTML
<p>Me <em>alegro</em> de que estés aquí.</p>
<p>Me <i>alegro</i> de que estés aquí (sin semántica).</p>
```

## **`<b>` VS `<strong>`**

- **`<strong>`**: Indica que el texto es de gran importancia y tiene significado semántico.
- **`<b>`**: Coloca el texto en negrita sin significado semántico. Se usa para resaltar texto sin indicar importancia.
```html
<p><strong>Advertencia:</strong> Este es un mensaje importante.</p>
<p><b>Nota:</b> Este es un mensaje destacado.</p>
```

## **`<q>`**: CITAS BREVES EN LÍNEA

- Se utiliza para citas cortas y generalmente agrega comillas automáticamente.
- El atributo `cite` proporciona una fuente para la cita, aunque no siempre se ve.

```html
<p>Según <q cite="https://ejemplo.com">esta fuente</q>, deberías intentarlo.</p>
```

## **`<blockquote>`**: CITAS LARGAS EN BLOQUE

- Usada para citas largas o bloques de texto citados de otra fuente.
- El atributo `cite` indica la fuente de la cita.

```html
<blockquote cite="https://ejemplo.com">
  <p>Esta es una cita larga, tomada de una fuente externa.</p>
</blockquote>
```

## **`<abbr>`**: ABREVIACIONES

- Se usa para abreviaciones o acrónimos y ofrece más información con el atributo `title`.
```html
<p>El término <abbr title="HyperText Markup Language">HTML</abbr> es fundamental para la web.</p>
```

## **`<del>` Y `<ins>`**: TEXTO TACHADO Y TEXTO INSERTADO

- **`<del>`**: Representa texto que ha sido eliminado o sustituido.
- **`<ins>`**: Representa texto que ha sido añadido.

```html
<p>El agua es <del>húmeda</del> insípida.</p>
<p>El agua es <ins>transparente</ins>.</p>
```

## **`<time>`**: FECHAS Y HORAS

- Se utiliza para marcar un período específico en el tiempo.
- El atributo `datetime` define una fecha y hora legible por el ordenador.

```html
<p>El evento será el <time datetime="2024-10-02">2 de octubre de 2024</time>.</p>
```

## **`<small>`**: TEXTO PEQUEÑO

- Reduce el tamaño del texto. En HTML5, también se utiliza para notas legales o aclaraciones.

```html
<p>Este texto es normal. <small>Este es más pequeño.</small></p>`
```

## **`<wbr>`**: PUNTO DE RUPTURA

- Inserta un posible punto de ruptura de palabra para ajustar la visualización en pantallas más pequeñas.

```html
<p>Esta palabra es muy larga<wbr> y puede romperse aquí si es necesario.</p>
```

## **`<hr>`**: SEPARADOR DE TEMAS

- Inserta una línea horizontal para marcar una separación temática.

```html
<hr>
<p>Esto es un cambio de tema.</p>
```

## **`<pre>`**: TEXTO PREFORMATEADO

- Preserva espacios y saltos de línea.
```html
<pre>
  Línea 1
    Línea 2 con sangría
  Línea 3
</pre>
```

## **`<address>`**: INFORMACIÓN DE CONTACTO

- Representa información de contacto del autor o propietario.

```html
<address>
  Contacto: <a href="mailto:webmaster@example.com">webmaster@example.com</a>
</address>
```


# ETIQUETAS DE INTERACCIÓN

## **`<kbd>`**: ENTRADA DEL USUARIO

- Representa la entrada del usuario, como atajos de teclado o comandos.
```html
<p>Presiona <kbd>Ctrl + S</kbd> para guardar el archivo.</p>`
```

## **`<samp>`**: SALIDA DEL SISTEMA

- Representa la salida del sistema, como los resultados de un programa.

```html
<p>Resultado: <samp>Error: archivo no encontrado</samp></p>`
```

## **`<code>`**: FRAGMENTOS DE CÓDIGO

- Muestra código fuente o texto que representa código.

```html
<p>Usa el comando <code>git status</code> para verificar el estado del repositorio.</p>
```


# IFRAME (`<iframe>`)

Permite incrustar otro documento HTML dentro de la página actual.

```html
<iframe src="www.ejemplo.com" width="600" height="400" title="Ejemplo"></iframe>
```

Atributos:

- **`src`**: URL del documento a incrustar.
- **`width`** y **`height`**: Dimensiones del iframe.
- **`title`**: Descripción del contenido para accesibilidad.
- **`sandbox`**: Aplica restricciones de seguridad.


# MULTIMEDIA ACCESIBLE

## **`<figure>` Y `<figcaption>`**

- **`<figure>`**: Agrupa contenido como imágenes, gráficos, etc., y su pie de foto.
- **`<figcaption>`**: Proporciona una leyenda o pie de foto, para mejorar la accesibilidad.

```html
<figure>
  <img src="imagen.jpg" alt="Descripción de la imagen">
  <figcaption>Este es el pie de foto que describe la imagen.</figcaption>
</figure>
```

# ATRIBUTOS DE DATOS PERSONALIZADOS (`data-*`)

Permiten almacenar información personalizada en elementos HTML.
  
```html
<div id="producto1" data-id="12345" data-precio="19.99">
  <p>Nombre del producto</p>
</div>
```

En JavaScript, puedes acceder a ellos mediante `dataset`:

```js
const producto = document.getElementById('producto1');
console.log(producto.dataset.id); // "12345"
console.log(producto.dataset.precio); // "19.99"
```

# ENLACES DE ANCLAJE

Permiten crear enlaces a secciones específicas dentro de una página.
```html
<!-- Enlace al elemento con id="seccion1" -->
<a href="#seccion1">Ir a la sección 1</a>

<!-- Elemento de destino -->
<h2 id="seccion1">Sección 1</h2>
```
- - - 
#html