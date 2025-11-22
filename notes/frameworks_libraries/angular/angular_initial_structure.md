# ANGULAR > INITIAL STRUCTURE 

These are files for configurations :

```
**tsconfig.json**  <-- root config file. Global compiler options,stricness, path aliases...
tsconfig.app.json <-- extends _| |  specifies which ts files are part of the app
tsconfig.scep.json <--   extends_| you can create custom tsconfigs for tests, etc... 
```

- [[nodejs_package.json|package.json]]
- `angular.json`: extra configuration settings for the angular cli, and the angular provided tools in general. tipically not needed to change.
- `.editorconfig`: ->  
- `main.ts` -> executes the function `bootstrapApplication` from @angular/platform-browser., wants a component, takes it, and looks for it in the index.html, and tries to replace it with a markup you define in the custom component.

>[!danger] NO .ts when importing typescript files

 - `styles.css` -> 
 - `index.html` ->  the body only contains a non standard html element. `<app-root>`
	- automaticly injected scripts that are not shown but are injected by the Angular CLI
 - `main.ts` -> first code file to be executed when the angular app launches



