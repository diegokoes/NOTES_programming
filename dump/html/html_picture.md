# HTML -> Picture

For adaptive images.
```html
<picture>
  <source srcset="image-large.png" media="(min-width: 800px)">
  <img src="image-small.png" alt="Description">
</picture>
```
 **`<source>` in `<picture>`**

- Within `<picture>`, the `media` attribute defines the condition that must be met to display a specific image.
- The `srcset` attribute allows defining different versions of the image, with higher or lower resolution to adapt to different screen densities.
```html
<picture>
  <source media="(min-width: 800px)" srcset="image-xl.png, image-xl-hd.png 2x">
  <source media="(min-width: 600px)" srcset="image-l.png, image-l-hd.png 2x">
  <img src="image-small.png" alt="Adaptive image">
</picture>
```
- - -
#html