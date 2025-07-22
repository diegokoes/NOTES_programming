### Definition

**Official:**
> A single-page application (SPA) is a web application or website that interacts with the user by dynamically rewriting the current web page with new data from the web server, instead of the default method of loading entire new pages. The goal is faster transitions that make the website feel more like a native app.
> In a SPA, a page refresh never occurs; instead, all necessary HTML, JavaScript, and CSS code is either retrieved by the browser with a single page load,or the appropriate resources are dynamically loaded and added to the page as necessary, usually in response to user actions.

**Personal:**
>Antes  cliente -> petición de una página html, server devuelve 1 página entera. Otra página? Otra petición y la devuelve. Mucho trasiego si hay muchos clientes.
>Ahora los clientes -> 1 petición (tocha), se descargan todo. Y toda la carga en el navegador. [Lazy Load](LazyLoad.md) + micro-llamadas.
- - - 
### ¿En qué se basan todos los PSA?
  - > [!info]- Click to reveal the answer
	 En programación por componentes

### ¿Cuántos html hay en una aplicación SPA?
  - > [!info]- Click to reveal the answer
    Una única: index.html. Se crea un **árbol de componentes**, con un componente root, del que se van quitando/poniendo el resto en función de la url que ponga el usuario en el navegador.

### ¿Qué parte de la app carga un componente u otro?
  - > [!info]- Click to reveal the answer
El módulo de enrutamiento.


!todo
### **SPA (Single-Page Application)**:

A SPA is a modern web application architecture where all the necessary resources (HTML, CSS, JavaScript) are loaded once, and the page is dynamically updated as the user interacts with the application. The browser does not reload the page during interactions; instead, it fetches data from the server as needed (usually via APIs) and updates the user interface accordingly.

**Key Features of a SPA:**

- Only a single HTML page is loaded at the start.
- User interactions do not trigger full page reloads; instead, content is dynamically loaded and rendered.
- Faster and more fluid user experience since only parts of the page are updated.
- More reliant on JavaScript frameworks (like React, Angular, Vue).
- Can be more challenging to optimize for SEO without special techniques (like server-side rendering or prerendering).

**Example**: Web applications like Gmail, Facebook, and many modern dashboards or content management systems use the SPA approach.
- - - 
#conceptsprotocols 