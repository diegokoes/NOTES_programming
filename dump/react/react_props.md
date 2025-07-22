# React
## Summary
> [!summary]


## Theory

## Questions
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer

- - - 
# ## Questions


#### ¿Cómo se pasan las props a un componente?

  - > [!info]- Click to reveal the answer
Se pasan como atributos en la etiqueta del componente, utilizando la sintaxis de JSX.```<Componente ejemplo="Valor" />```

#### ¿Cómo se acceden a las props dentro de un componente?

  - > [!info]- Click to reveal the answer
En componentes funcionales, las props se reciben como argumento de la función. En componentes de clase, se acceden con this.props.```function Saludo(props){return <h1>hola, {props.nombre}</h1>```

#### ¿Son las props modificables?

  - > [!info]- Click to reveal the answer
No, las props son inmutables. Esto significa que no se pueden cambiar dentro del componente que las recibe.

#### ¿Cuál es la diferencia entre props y state?

  - > [!info]- Click to reveal the answer
**Props**: Se pasan desde un componente padre y son inmutables.
**State**: Es local al componente y puede cambiar con el tiempo.

#### ¿Qué pasa si no se pasa una prop requerida?

  - > [!info]- Click to reveal the answer
Si no se pasa una prop, su valor será undefined a menos que el componente tenga un valor predeterminado configurado.

- - - 
#react 



