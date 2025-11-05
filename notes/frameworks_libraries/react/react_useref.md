# react > useRef()

## SUMMARY
> [!summary]
>
## THEORY
Should be called at the top level of the component.

useRef is a [[[reacthooks|hook]] that returns an [[oop_object|object]] *with a single property* => **current**, that you can set to an initial value when calling the function.
```jsx
function Example() {
	const intervalRef = useRef(9);
}
```
The value of current **doesn't change between renders** and more importantly, it **doesn't trigger a re-render**. 

What can be stored in it? : **EVERYTHING** 

What is it commonly used for? :  

> [!danger] TO AVOID


## QUESTIONS

> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer

