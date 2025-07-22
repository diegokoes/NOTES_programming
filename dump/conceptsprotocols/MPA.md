# MPA
## Summary
> [!summary]-
> 
- - - 

**Personal:**
>Una MPA es una aplicación web tradicional donde, al hacer clic en un enlace o enviar un formulario, el navegador carga una nueva página desde el servidor, reemplazando completamente el contenido de la anterior.

- - - 
## Theory

## Questions
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer.
## ##  **Preguntas y Respuestas**

1. **¿Cómo funciona una MPA?**
    
    - Cada página de la aplicación está compuesta por un archivo HTML independiente.
    - El navegador solicita al servidor una nueva página completa cada vez que el usuario navega o interactúa.
    - La lógica de negocio y el renderizado suelen realizarse en el servidor (Server-Side Rendering, SSR).
2. **¿Cuál es la diferencia entre una MPA y una SPA?**
    
    - En una MPA, cada interacción implica recargar la página; en una SPA, el contenido se actualiza dinámicamente sin recargar.
    - Las MPAs dependen más del servidor para construir las páginas, mientras que las SPAs delegan gran parte de la lógica al cliente.
3. **¿Qué ventajas tiene una MPA?**
    
    - **SEO óptimo:** Cada página tiene su propio URL, lo que facilita el indexado por motores de búsqueda.
    - **Simplicidad inicial:** Más fácil de desarrollar si la aplicación no requiere demasiada interactividad.
    - **Compatibilidad amplia:** Funciona bien en navegadores antiguos sin necesidad de librerías o frameworks modernos.
4. **¿Cuáles son las desventajas de una MPA?**
    
    - **Navegación más lenta:** La carga completa de páginas puede ser más lenta en comparación con una SPA.
    - **Mayor consumo de recursos del servidor:** El servidor debe renderizar cada página completa.
    - **Complejidad en el estado de la aplicación:** Manejar datos compartidos entre páginas puede requerir trabajo adicional.

---

## ##  **Ejemplo Práctico de MPA**

## ## ##  Proceso típico de una MPA:

1. El usuario accede a la URL `www.ejemplo.com/inicio`.
2. El servidor envía el HTML completo de la página de inicio.
3. El usuario hace clic en "Contacto" → El navegador solicita al servidor la URL `www.ejemplo.com/contacto`.
4. El servidor envía otro archivo HTML completo para la página de contacto.

---

## ##  **Ventajas Técnicas de las MPAs**

1. **Integración Natural con SSR:**
    
    - Se adapta bien a aplicaciones donde los datos dinámicos deben generarse en el servidor.
2. **Estructura Sencilla:**
    
    - Cada página tiene su propio HTML, lo que facilita la división lógica.
3. **Seguridad:**
    
    - Como las interacciones son manejadas mayormente en el servidor, es más fácil proteger las rutas y evitar manipulaciones del lado del cliente.

---

## ##  **Cuándo Usar una MPA**

1. **Aplicaciones centradas en contenido:**
    
    - Blogs, portales de noticias, sitios de e-commerce donde el SEO es fundamental.
2. **Aplicaciones con poca interactividad:**
    
    - Sitios donde los usuarios principalmente navegan y consumen contenido estático.
3. **Proyectos simples o pequeños:**
    
    - Cuando no se requiere un manejo complejo de estado o actualizaciones dinámicas.
- - - 
#conceptsprotocols 