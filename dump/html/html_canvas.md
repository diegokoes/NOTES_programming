Allows drawing graphics via scripting (usually JavaScript).
```html 
<canvas id="myCanvas" width="200" height="100"></canvas>
```

In JavaScript:
```javascript
const canvas = document.getElementById('myCanvas');
const context = canvas.getContext('2d');
context.fillStyle = 'blue';
context.fillRect(0, 0, 200, 100);
```