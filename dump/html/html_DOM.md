# DOM (Document Object Model)

## Summary

> [!summary]
> The Document Object Model (DOM) is a programming interface for HTML and XML documents that represents the page as a tree-structured hierarchy of nodes. It allows JavaScript to access, manipulate, and update the content, structure, and style of web documents dynamically. The DOM serves as a bridge between web pages and programming languages, enabling interactive web applications by providing methods and properties to interact with every element on a page.

## Theory

### What is the DOM?

The DOM represents an HTML document as a hierarchical tree structure where each part of the document (elements, attributes, text) becomes a node in the tree. This representation allows scripts to:

- Access and manipulate any element in the document
- Modify content and visual presentation
- Respond to user interactions
- Create dynamic content without page reloads

### DOM Tree Structure

- **Document node**: The root of the DOM tree
- **Element nodes**: HTML elements like `<div>`, `<p>`, `<body>`
- **Attribute nodes**: Attributes of HTML elements
- **Text nodes**: Text content inside elements
- **Comment nodes**: HTML comments

### DOM Creation Process

1. The browser receives HTML content
2. The parser processes HTML tags and creates a DOM tree
3. CSS is processed by the CSSOM (CSS Object Model)
4. The render tree combines DOM and CSSOM
5. Layout calculation (reflow) determines element positions
6. Visual display (repaint) draws elements on screen

### DOM Traversal

```javascript
// Parent relationships
element.parentNode
element.parentElement

// Child relationships
element.childNodes       // All child nodes (including text nodes)
element.children         // Element children only
element.firstChild       // First child (may be text node)
element.firstElementChild // First element child
element.lastChild        // Last child
element.lastElementChild // Last element child

// Sibling relationships
element.nextSibling      // Next sibling (may be text node)
element.nextElementSibling // Next element sibling
element.previousSibling  // Previous sibling
element.previousElementSibling // Previous element sibling
```

### DOM Selection Methods

```javascript
// Single element selectors
document.getElementById('id')
document.querySelector('selector')

// Multiple elements selectors
document.getElementsByClassName('class')
document.getElementsByTagName('tag')
document.querySelectorAll('selector')
```

### DOM Manipulation

```javascript
// Creating elements
const element = document.createElement('div')

// Modifying content
element.textContent = 'Text'  // Text only (safer)
element.innerHTML = '<span>HTML</span>'  // With HTML (security risk)

// Attributes
element.setAttribute('attr', 'value')
element.getAttribute('attr')
element.removeAttribute('attr')
element.hasAttribute('attr')

// Classes
element.classList.add('class')
element.classList.remove('class')
element.classList.toggle('class')
element.classList.contains('class')

// Styles
element.style.property = 'value'

// DOM insertion
parent.appendChild(element)
parent.insertBefore(element, referenceNode)
parent.replaceChild(newElement, oldElement)
parent.removeChild(element)

// Modern insertion methods
parent.append(...nodes)           // Append at end
parent.prepend(...nodes)          // Insert at beginning
element.before(...nodes)          // Insert before element
element.after(...nodes)           // Insert after element
element.replaceWith(...nodes)     // Replace element
```

### Events

```javascript
// Adding event listeners
element.addEventListener('event', callback)
element.removeEventListener('event', callback)

// Event object properties
event.target            // Element that triggered event
event.currentTarget     // Element event is attached to
event.preventDefault()  // Prevent default action
event.stopPropagation() // Stop event bubbling/capturing
```

### Performance Considerations

- DOM manipulation is expensive for performance
- Batch DOM updates when possible
- Minimize reflows and repaints
- Use document fragments for multiple insertions
- Consider virtual DOM libraries for complex applications

## Questions

> [!tip]- What's the difference between textContent and innerHTML?
> **textContent** sets or returns the text content of a node and all its descendants, treating HTML as plain text (safer, prevents XSS attacks). **innerHTML** sets or returns the HTML content inside an element, parsing and rendering any HTML tags (potentially unsafe if using user input).

> [!tip]- How do you select elements in the DOM?
> You can select elements using several methods:
>
> - `document.getElementById('id')` - selects by ID (fastest)
> - `document.getElementsByClassName('class')` - returns a live HTMLCollection of elements with the specified class
> - `document.getElementsByTagName('tag')` - returns elements with the specified tag
> - `document.querySelector('selector')` - returns the first element matching the CSS selector
> - `document.querySelectorAll('selector')` - returns all elements matching the CSS selector as a static NodeList

> [!warning]- What is event delegation and why is it useful?
> **Event delegation** is a technique where you attach a single event listener to a parent element instead of multiple listeners on individual child elements. It leverages event bubbling to handle events for multiple elements with a single handler.
> 
> **Benefits**:
> - Improved memory efficiency (fewer event listeners)
> - Automatically handles dynamically added elements
> - Reduces code complexity for many similar elements
> 
> **Example**:
> ```javascript
> document.getElementById('parent-list').addEventListener('click', function(e) {
>   if (e.target.tagName === 'LI') {
>     // Handle click on list item
>   }
> });
> ```

> [!warning]- How do browser reflows and repaints affect performance?
> **Reflow** (or layout) happens when the browser needs to recalculate the positions and dimensions of elements in the document. **Repaint** occurs when changes are made to an element's appearance without changing its layout.
> 
> Reflows are more expensive than repaints, as they require the entire document flow to be recalculated. Common causes include:
> - Changing element dimensions (width, height)
> - Adding/removing DOM elements
> - Changing font size or font family
> - Scrolling or resizing the window
> 
> To optimize performance:
> - Batch DOM updates
> - Modify classes instead of inline styles
> - Use document fragments for multiple insertions
> - Use `position: absolute` or `fixed` for elements that move frequently
> - Minimize accessing layout properties (offsetHeight, scrollTop, etc.)

> [!danger]- How would you implement a custom event system that mimics the DOM event system?
> A custom event system would need:
> 
> ```javascript
> class EventEmitter {
>   constructor() {
>     this.events = {};
>   }
>   
>   on(eventName, callback) {
>     if (!this.events[eventName]) {
>       this.events[eventName] = [];
>     }
>     this.events[eventName].push(callback);
>     return this; // For method chaining
>   }
>   
>   off(eventName, callback) {
>     if (this.events[eventName]) {
>       this.events[eventName] = this.events[eventName]
>         .filter(handler => handler !== callback);
>     }
>     return this;
>   }
>   
>   emit(eventName, ...args) {
>     const event = { type: eventName, target: this };
>     if (this.events[eventName]) {
>       this.events[eventName].forEach(callback => {
>         callback.apply(this, [event, ...args]);
>       });
>     }
>     return this;
>   }
>   
>   once(eventName, callback) {
>     const onceWrapper = (...args) => {
>       callback(...args);
>       this.off(eventName, onceWrapper);
>     };
>     return this.on(eventName, onceWrapper);
>   }
> }
> ```
> 
> This implementation includes standard event methods like `on`, `off`, `emit`, and `once`, similar to how DOM events work but with a more streamlined API.

> [!danger]- What are Shadow DOM and Virtual DOM, and how do they differ from the regular DOM?
> **Shadow DOM**:
> - Browser-native technology for encapsulating DOM trees
> - Creates isolated DOM subtrees attached to elements
> - Scopes CSS styles to prevent leaking
> - Used in Web Components to create reusable custom elements
> - Example: `element.attachShadow({ mode: 'open' })`
> 
> **Virtual DOM**:
> - Programming concept used by libraries like React
> - Lightweight JavaScript representation of the actual DOM
> - Changes are first applied to the virtual DOM, then efficiently synced with the real DOM
> - Minimizes expensive DOM operations by batching updates
> - Not a browser standard but a design pattern
> 
> **Main differences**:
> - Shadow DOM is a web standard for DOM encapsulation; Virtual DOM is a programming concept
> - Shadow DOM exists in the actual DOM; Virtual DOM exists only in memory
> - Shadow DOM focuses on style and markup isolation; Virtual DOM focuses on performance optimization

- - - 
#conceptsprotocols