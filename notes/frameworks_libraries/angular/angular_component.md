# ANGULAR > COMPONENTS

## SUMMARY
> [!summary]
> 
## THEORY

```ts
import {Component} /* THE DECORATOR*/ from '@angular/core'

@Component({
	 // selector: 'header' //!AVOID ONE WORD SELECTORS - CAN CLASH WITH HTML
	selector: 'app-header',
	standalone: true, // <--- no need to specify it in Angular 19+
	//template: 'HTML INSIDE' <-- AVOID IT
	imports: [ ]
	templateUrl: './header-component.html'
	//styles: ['array  of inline  styles' ] <-- AVOID IT
	styleUrl: './header-component.css'
	//styleUrls: [''.'']
	
})
export class HeaderComponent
```

![[Pasted image 20251122124748.png]]

All the properties in the component class are avaliable in the template of that component.
### TYPES OF COMPONENTS
//TODO EXPAND
- module based components
- standalone based components: easier to use and tie together

### STRING INTERPOLATION
### PROPERTY BINDING

when to do it?

Attribute Binding

In the previous lecture, you were introduced to **"Property Binding"** - a key Angular feature that allows you to bind element properties to dynamic values.

For example, `<img [src]="someSrc">` binds the `src` property of the underlying [HTMLImageElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement) DOM object to the value stored in `someSrc`.

Whilst it might look like you're binding the src attribute of the `<img>` tag, you're actually NOT doing that. Instead, property binding really targets the underlying DOM object property (in this case a property that's also called `src`) and binds that.

This might look like a subtle detail (and often it indeed doesn't matter) but it's important to understand this difference between element attributes and property. [This article](https://jakearchibald.com/2024/attributes-vs-properties/) can help with understanding this difference.

Whilst it won't make a difference in Angular apps in many cases, it DOES matter if you're trying to set attributes on elements dynamically. Attributes which don't have an equally-named underlying property.

For example, when binding [ARIA attributes](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA), you can't target an underlying DOM object property.

Since "Property Binding" wants to target properties (and not attributes), that can be a problem. That's why Angular offers a slight variation of the "Property Binding" syntax that does allow you to bind attributes to dynamic values.

It looks like this:
```ts
1.   <div 
2.   role="progressbar" 
3.   [attr.aria-valuenow]="currentVal" 
4.   [attr.aria-valuemax]="maxVal">...</div>
```

By adding `attr` in front of the attribute name you want to bind dynamically, you're _"telling"_ Angular that it shouldn't try to find a property with the specified name but instead bind the respective attribute - in the example above, the `aria-valuenow` and `aria-valuemax` attributes would be bound dynamically.


### CHANGE DETECTION

`zone.js` under the hood.
![[Pasted image 20251122152237.png]]

Listen to all possible events, or a timer expiring, and then checks the possible changes in the application


![[Pasted image 20251122152430.png]]![[Pasted image 20251122152600.png]]

@Inputs -> PROPS

also can accept inputs from a signal. Input and input different.

for what inputs with signals? -> efficient

