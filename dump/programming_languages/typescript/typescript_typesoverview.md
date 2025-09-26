# TYPESCRIPT -> TYPES OVERVIEW

## SUMMARY
>
> [!summary]
>In TypeScript, “types” are annotations that allow you to explicitly define the>structure and nature of data for variables, function parameters, and return values. By enforcing these rules, TypeScript helps detect errors at compile time, making code more reliable and maintainable.
>
> A

## THEORY

- [[typescript_basic_types|Basic Types]]
- [[typescript_objects|Objects]]
- [[typescript_functions|Functions]]
- [[typescript_optional_types|Optional Types]]
- [[typescript_assertioncasting|Type Assertions and Casting]]

- **Advanced Types**
 	- [[typescript_unionintersection|Union and Intersection Types]]
 	- [[typescript_literaltypes|Literal Types]]
 	- [[typescript_mappedtypes|Mapped Types]]
 	- [[typescript_conditionaltypes|Conditional Types]]
 	- [[typescript_utilitytypes|Utility Types]]
 	- [[typescript_record|Record Type]]
 	- [[typescript_custom_types|Custom Types]]
  - [[typescript_generics|Generics]]

### INFERENCE

TypeScript inferences the type automaticly if it isn't specified.

```ts
let age = 30; // TypeScript inferes that it is a number
```

#### BEST COMMON TYPE
>
> [!summary]
>Infers the type by what's assigned to it

When there are multiple expressions to inference from, the `best common type`algorithm considers each element, and picks the one that is compatible with other elements.

Here because there is not super type, the type will be the **union** of all types in the array:

```ts
let zoo = [ new Rhino(), new Elephant(), new Snake() ];
  // type-> let zoo: (Rhino | Elephant | Snake)[]
```

If we explicitly provide the type of the array it changes the type, because it is compatible with all the other elements:

```ts
let zoo: Animal[] = [new Rhino(), new Elephant(), new Snake() ];
  // type-> let zoo: Animal[]
```

#### CONTEXTUAL TYPING
>
> [!summary]
> Infers the type by the **location** where a variable is placed

The function assigned to window.onmousedown is expected to handle mouse events. TypeScript infers that mouseEvent is of type MouseEvent based on this context.

Attempting to access `mouseEvent.kangaroo` results in a compile-time error, preventing potential runtime bugs by ensuring only valid properties are accessed.

```ts
window.onmousedown = function (mouseEvent) {
  console.log(mouseEvent.button);
  console.log(mouseEvent.kangaroo);
// Property 'kangaroo' does not exist on type 'MouseEvent'.
};
```

## QUESTIONS
>
> [!tip]- What is a type annotation in TypeScript?
> A type annotation is an explicit way to specify the data type of a variable, function parameter, or return value.
>
> ```typescript
> let userName: string = "Alice"; // Explicitly typed as string
> ```

> [!warning]- What happens if you assign the wrong type to a variable in TypeScript?
> The TypeScript compiler will show an error at compile time, preventing the code from compiling successfully.
>
> ```typescript
> let age: number = "30";
// Error: Type 'string' is not assignable to type 'number'
>
>```

> [!warning]- How does TypeScript’s structural typing work?
> Instead of matching types by name, TypeScript compares the shape of the objects or interfaces. If they match structurally, they are considered compatible.

> [!danger]- How can you use custom types to define complex structures?
> By using `type` or `interface`, you can create specific shapes for complex objects, ensuring consistent usage throughout your codebase.
>
>```typescript
>type Order = {
  id: string;
  total: number;
  items: string[];
};
>let newOrder: Order = { id: "abc123", total: 99, items: ["item1", "item2"] };
>
>```
>
- - -
# TYPESCRIPT
