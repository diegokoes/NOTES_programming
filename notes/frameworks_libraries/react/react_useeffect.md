# DEFINITION

**Official:**  
> The `useEffect` hook in React lets you perform side effects in function components. It serves purposes like data fetching, subscriptions, or manually changing the DOM.

**Personal:**  
> `useEffect` is like a "watcher" for your React components. It runs specific code (a side effect) when the component renders or when certain values change.

---

# THEORY

## B.1- UNDERSTANDING `useEffect`

The `useEffect` hook replaces lifecycle methods like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in class components.

### BASIC SYNTAX

```javascript
> import { useEffect } from "react";
>
> useEffect(() => {
>   // Code to run after the component renders
> });
```

### **WHEN DOES `useEffect` RUN?**
1. After every render by default.
2. On changes to specific values if dependencies are provided.
3. When cleaning up after a component unmounts or before re-running an effect.

---

## B.2- DEPENDENCY ARRAY

The second argument to `useEffect` is the **dependency array**, which controls when the effect should run.

1. **No Dependency Array:**  
   Runs after every render.
   ```javascript
   > useEffect(() => {
   >   console.log("Rendered!");
   > });
   ```

2. **Empty Dependency Array:**  
   Runs only once after the initial render.
   ```javascript
   > useEffect(() => {
   >   console.log("Mounted!");
   > }, []);
   ```

3. **Specific Dependencies:**  
   Runs only when the specified dependencies change.
   ```javascript
   > useEffect(() => {
   >   console.log("Count updated!");
   > }, [count]);
   ```

---

## B.3- CLEANUP IN `useEffect`

The `useEffect` function can return a cleanup function. This is useful for cleaning up subscriptions, timers, or other resources.

1. **Basic Cleanup Example:**  
   ```javascript
   > useEffect(() => {
   >   const interval = setInterval(() => {
   >     console.log("Running interval");
   >   }, 1000);
   >
   >   return () => clearInterval(interval); // Cleanup
   > }, []);
   ```

2. **Event Listener Cleanup:**  
   ```javascript
   > useEffect(() => {
   >   const handleScroll = () => console.log("Scrolling!");
   >   window.addEventListener("scroll", handleScroll);
   >
   >   return () => window.removeEventListener("scroll", handleScroll);
   > }, []);
   ```

---

## B.4- COMMON PATTERNS WITH `useEffect`

1. **Data Fetching:**  
   ```javascript
   > useEffect(() => {
   >   fetch("https://api.example.com/data")
   >     .then((response) => response.json())
   >     .then((data) => console.log(data));
   > }, []);
   ```

2. **Subscribing to External Data:**  
   ```javascript
   > useEffect(() => {
   >   const unsubscribe = someAPI.subscribe((data) => console.log(data));
   >
   >   return () => unsubscribe(); // Cleanup
   > }, []);
   ```

3. **Conditional Logic in Dependencies:**  
   ```javascript
   > useEffect(() => {
   >   if (isActive) {
   >     console.log("Effect ran because isActive changed!");
   >   }
   > }, [isActive]);
   ```

---

# QUESTIONS

> [!tip]- **What is the purpose of `useEffect` in React?**  
> It allows you to perform side effects like fetching data, subscribing to events, or manipulating the DOM after a component renders.

> [!tip]- **How can you make an effect run only once?**  
> By passing an empty dependency array `[]` to the `useEffect` hook.  
> ```javascript
> useEffect(() => {
>   console.log("Only runs on mount!");
> }, []);
> ```

> [!question]- **What happens if you omit the dependency array?**  
> The effect will run eafter every render.  
> ```javascript
> useEffect(() => {
>   console.log("Runs after every render");
> });
> ```

> [!question]- **What is the purpose of the cleanup function in `useEffect`?**  
> To clean up resources like timers, subscriptions, or event listeners when the component unmounts or before re-running the effect.  
> ```javascript
> useEffect(() => {
>   const timer = setTimeout(() => console.log("Timer!"), 1000);
>   return () => clearTimeout(timer);
> });
> ```

> [!warning]- **What happens if you add an empty dependency array but forget cleanup?**  
> Resources like timers or subscriptions might leak, causing memory issues.  
> ```javascript
> useEffect(() => {
>   const interval = setInterval(() => console.log("Interval"), 1000);
> }, []); // No cleanup!
> ```

> [!warning]- **How do you handle asynchronous operations in `useEffect`?**  
> Use an async function inside the effect and call it directly. Avoid making `useEffect` itself async.  
> ```javascript
> useEffect(() => {
>   const fetchData = async () => {
>     const response = await fetch("https://api.example.com/data");
>     console.log(await response.json());
>   };
>   fetchData();
> }, []);
> ```

> [!danger]- **Why should you avoid leaving out dependencies?**  
> It can cause stale closures, leading to incorrect logic or missed updates.  
> ```javascript
> useEffect(() => {
>   console.log("Count:", count); // Might not get the latest value
> }, []); // Missing count dependency
> ```

> [!danger]- **How can you optimize `useEffect` usage for better performance?**  
> Minimize dependencies, clean up resources properly, and avoid heavy computations inside the effect.  
> ```javascript
> useEffect(() => {
>   const expensiveOperation = () => console.log("Heavy computation");
>   expensiveOperation();
> }, [dependency]);
> ```

