# react > useEffect
## SUMMARY

> [!summary] 
> `useEffect` is like a "watcher" for React components. It runs specific code (a side effect) when the component renders or when certain values change.
## THEORY
### Parameters

**1. Effect Function (required)**

- Must be a **synchronous** function **without** arguments
- **Cannot be async** (must not return a Promise)
- Can optionally return a cleanup function

```js
useEffect(() => {
  // EFFECT CODE RUNS HERE (synchronous)

  // OPTIONAL: Return a cleanup function
  return () => {
    // CLEANUP CODE RUNS HERE
  }
});
```

**2. Dependency Array (optional)**

- Controls when the effect runs

---

### When Does the Effect Run?

- **No dependency array** → After every re-render
- **Empty dependency array `[]`** → Only on initial render
- **With dependencies `[dep1, dep2]`** → After those dependencies change

---

### Cleanup Function

**What is it?**
An optional function you return from your effect that cleans up resources (timers, listeners, subscriptions, etc.)

**When does cleanup run?**

1. **Before the effect runs again** (if dependencies changed)
2. **When the component unmounts** (is destroyed/removed from DOM)

**Why use it?**
To prevent memory leaks and unwanted behavior by cleaning up:

- Event listeners
- Timers/intervals
- Aborted fetch requests
- WebSocket connections

---

### Async Operations in useEffect

**Important:** The effect function itself **must be synchronous**, but you can call async functions inside it.

**Two approaches for async operations:**

**Approach 1: Using .then/.catch (preferred for simple cases)**

```javascript
useEffect(() => {
  fetch("https://api.example.com/data")
    .then((response) => response.json())
    .then((data) => console.log(data))
    .catch((error) => console.error("Fetch error:", error));
}, []);
```

**Approach 2: Define and call async function inside effect**

```javascript
useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await fetch("https://api.example.com/data");
      const data = await response.json();
      console.log(data);
    } catch (error) {
      console.error("Fetch error:", error);
    }
  };

  fetchData(); // Call the async function
}, []);
```

---

### Common Examples

**1. Timer Cleanup**

```javascript
useEffect(() => {
  const interval = setInterval(() => {
    console.log("Running interval");
  }, 1000);

  return () => clearInterval(interval); // Cleanup runs on unmount
}, []);
```

**2. Event Listener Cleanup**

```javascript
useEffect(() => {
  const handleScroll = () => console.log("Scrolling!");
  window.addEventListener("scroll", handleScroll);

  return () => window.removeEventListener("scroll", handleScroll);
}, []);
```

**3. Abortable Fetch with .then/.catch (with cleanup)**

```javascript
useEffect(() => {
  const controller = new AbortController();

  fetch("https://api.example.com/data", { signal: controller.signal })
    .then((response) => response.json())
    .then((data) => console.log(data))
    .catch((error) => {
      if (error.name !== 'AbortError') {
        console.error("Fetch error:", error);
      }
    });

  return () => controller.abort(); // Cleanup: abort fetch if component unmounts
}, []);
```

**4. Abortable Fetch with async/await (with cleanup)**

```javascript
useEffect(() => {
  const controller = new AbortController();

  const fetchData = async () => {
    try {
      const response = await fetch("https://api.example.com/data", {
        signal: controller.signal
      });
      const data = await response.json();
      console.log(data);
    } catch (error) {
      if (error.name !== 'AbortError') {
        console.error("Fetch error:", error);
      }
    }
  };

  fetchData();

  return () => controller.abort(); // Cleanup
}, []);
```

**5. Conditional Logic with Dependencies**

```javascript
useEffect(() => {
  if (isActive) {
    console.log("Effect ran because isActive changed!");
  }
}, [isActive]);
```

---

## QUESTIONS

> [!danger]- **Why can't the effect function itself be async?**
> `useEffect` expects either nothing or a cleanup function to be returned. An async function always returns a Promise, not a cleanup function. This would break React's cleanup mechanism.
>
> ```javascript
> // ❌ WRONG - Effect function is async (returns a Promise)
> useEffect(async () => {
>   const response = await fetch("url");
>   const data = await response.json();
> }, []);
>
> // ✅ CORRECT - Effect function is sync, but calls async function inside
> useEffect(() => {
>   const fetchData = async () => {
>     const response = await fetch("url");
>     const data = await response.json();
>   };
>   fetchData();
> }, []);
>
> // ✅ ALSO CORRECT - Using .then/.catch (no async needed)
> useEffect(() => {
>   fetch("url")
>     .then(response => response.json())
>     .then(data => console.log(data));
> }, []);
> ```

