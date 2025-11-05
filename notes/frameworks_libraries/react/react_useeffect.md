# react > useEffect
## SUMMARY
> [!summary]
> `useEffect` is like a "watcher" for your React components. It runs specific code (a side effect) when the component renders or when certain values change.

## THEORY

**Use:** 

**Parameters:**
> 1. function logic + [return a cleanup function]


>2. \[ [dependency array] \] 


**When does the Effect run:** 

- No dependency array -> After every re-render.
- Empty dependency array -> Only initial render.
- Dependencies -> After they change



?? rerender  When cleaning up after a component unmounts or before re-running an effect.

###  CLEANUP IN `useEffect`

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

