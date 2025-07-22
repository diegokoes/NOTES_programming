# Concepts -> REST

Un **servicio REST** (Representational State Transfer) es un tipo de arquitectura de software utilizada para diseñar servicios web que permiten la comunicación e intercambio de datos entre diferentes sistemas a través de la web. REST se basa en un conjunto de principios y restricciones que facilitan la creación de servicios escalables, eficientes y fáciles de mantener.

### Principios Clave de REST

1. **Cliente-Servidor**: La arquitectura REST sigue una separación clara entre el cliente (que solicita recursos) y el servidor (que los provee). Esto permite que ambos evolucionen de manera independiente.
    
2. **Sin Estado (Stateless)**: Cada solicitud del cliente al servidor debe contener toda la información necesaria para entender y procesar la petición. El servidor no almacena ningún estado de sesión entre las solicitudes.
    
3. **Cacheable**: Las respuestas deben definir si son cacheables o no, lo que permite mejorar la eficiencia y reducir la carga en el servidor mediante el almacenamiento en caché de respuestas comunes.
    
4. **Interfaz Uniforme**: REST define una interfaz uniforme entre los componentes, simplificando y desacoplando la arquitectura. Esto incluye el uso de métodos HTTP estándar (como GET, POST, PUT, DELETE) y la manipulación de recursos a través de URLs.
    
5. **Sistema en Capas**: La arquitectura puede estar compuesta por varias capas (por ejemplo, servidores proxy, balanceadores de carga) que no afectan la interacción entre el cliente y el servidor final.
    
6. **Código Bajo Demanda (Opcional)**: Los servidores pueden proporcionar código ejecutable (como scripts JavaScript) que el cliente puede usar para extender su funcionalidad.
    

### Funcionamiento de un Servicio REST

- **Recursos**: En REST, los datos y funcionalidades se representan como recursos, identificados por URLs únicas. Por ejemplo, `https://api.ejemplo.com/usuarios/123` podría representar al usuario con ID 123.
    
- **Métodos HTTP**:
    
    - **GET**: Recupera información sobre un recurso sin modificarlo.
    - **POST**: Crea un nuevo recurso.
    - **PUT**: Actualiza un recurso existente.
    - **DELETE**: Elimina un recurso.
- **Formato de Datos**: Los servicios REST suelen utilizar formatos de datos como JSON o XML para la representación de recursos, siendo JSON el más popular debido a su ligereza y facilidad de uso.
    

### Ventajas de Usar Servicios REST

- **Escalabilidad**: La separación de cliente y servidor y la naturaleza sin estado permiten que los servicios REST escalen fácilmente.
- **Flexibilidad**: Se pueden consumir desde diversas plataformas y lenguajes de programación.
- **Simplicidad**: Aprovechan los estándares web existentes, lo que facilita su implementación y comprensión.
- **Mantenibilidad**: La arquitectura modular facilita las actualizaciones y el mantenimiento de los servicios.

### Ejemplo de una Interacción REST

Imaginemos una API REST para gestionar una lista de tareas:

- **Obtener todas las tareas**:
    
    - **Solicitud**: `GET https://api.ejemplo.com/tareas`
    - **Respuesta**: Lista de tareas en formato JSON.
- **Crear una nueva tarea**:
    
    - **Solicitud**: `POST https://api.ejemplo.com/tareas` con el cuerpo de la solicitud conteniendo los detalles de la tarea.
    - **Respuesta**: Confirmación de la creación con el ID de la nueva tarea.
- **Actualizar una tarea existente**:
    
    - **Solicitud**: `PUT https://api.ejemplo.com/tareas/123` con los datos actualizados.
    - **Respuesta**: Confirmación de la actualización.
- **Eliminar una tarea**:
    
    - **Solicitud**: `DELETE https://api.ejemplo.com/tareas/123`
    - **Respuesta**: Confirmación de la eliminación.

### Conclusión

Un servicio REST es una manera eficiente y estandarizada de construir APIs que permiten la comunicación entre diferentes sistemas a través de la web. Al seguir los principios de REST, se logra una arquitectura robusta, escalable y fácil de mantener, lo que lo convierte en una opción popular para el desarrollo de servicios web modernos.
- - - 
#conceptsprotocols 