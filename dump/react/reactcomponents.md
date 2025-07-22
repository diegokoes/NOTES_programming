# React -> Components
1. [Functional components](componentes_funcionales.md)
2. [Class components](componentes_clase.md)
3. [Props](react_props.md)
4. [State](state.md)
5. [Component lifecycle](ciclo_vida_componentes.md)
6. [Events](eventosReact.md)
7. [Conditional rendering](renderizado_condicional.md)
8. [List rendering](renderizado_listas.md)
- - - 
### Definition
**Official:**
> Un componente en React es una pieza reutilizable de código que representa una parte de la interfaz de usuario. Los componentes son independientes y gestionan su propio estado o reciben datos a través de propiedades (props).

**Personal:**
>Un componente es como una "pieza de Lego" en React. Sirve para construir la interfaz dividiendo la aplicación en partes más pequeñas y manejables, cada una con su funcionalidad específica. Puede ser tan simple como un botón o tan complejo como un formulario entero.

- - - 
#### ¿Qué son?
> [!info]- Click to reveal the answer
Una función javascript, generalmente el nombre de la función es el nombre del componente.

#### Esa función... ¿function name() o lambda?
> [!info]- Click to reveal the answer
```function() nombreFuncion(){ return ( < > . . . < / > ) }```. 



#### De qué están formados
> [!info]- Click to reveal the answer
  [PROPS](react_props.md), [STATE](react_state.md)código funcional y return de código [JSX](react_jsx.md)
#### **¿Cómo se comunican los componentes entre sí?**
> [!info]- Click to reveal the answer
La comunicación se hace a través de props, que permiten pasar datos de un componente padre a uno hijo. Para enviar datos de un hijo a un padre, se pueden usar funciones callback pasadas como props.
- - - 
#react 



