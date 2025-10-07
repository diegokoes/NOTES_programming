# DEFINITION

**Official Definition:** Angular architecture is the design framework that underpins the structure and functionality of Angular applications, built around modules, components, templates, services, and dependency injection. It enables the development of scalable and maintainable single-page applications (SPAs).

**Personal Definition:** Angular architecture organizes an application into small, manageable units like components and modules. It integrates powerful features such as routing, dependency injection, and zone-based change detection to enhance scalability and user experience.

---

# THEORY

## **1. MAIN ENTRY POINT: `main.ts`**

`main.ts` is the entry point of an Angular application. It initializes the application by bootstrapping the root module or component. Example:

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { appConfig } from './app/app.config';
import { AppComponent } from './app/components/zonaTienda/layoutComponents/app.component';

bootstrapApplication(AppComponent, appConfig)
  .catch((err) => console.error(err));
```

- **`bootstrapApplication(AppComponent, appConfig)`**:
  - `AppComponent`: The root component.
  - `appConfig`: Defines the application configuration (e.g., routing, dependency injection).

---

## **2. APP CONFIGURATION: `app.config.ts`**

The `app.config.ts` file defines an object implementing `ApplicationConfig`. This controls core settings like routing and dependency injection.

**Example:**

```typescript
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';

import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes)
  ]
};
```

- **`provideZoneChangeDetection`**: Optimizes change detection.
- **`provideRouter(routes)`**: Configures the routing module.

---

## **3. ROOT COMPONENT: `AppComponent`**

**Structure:**

```typescript
@Component({
  selector: 'app-root',
  imports: [RouterOutlet, HeaderComponent, FooterComponent],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'miebay';
}
```

- **Decorator `@Component`**:
  - Defines metadata for the component.
  - **`selector`**: The tag used in `index.html` to load the component.
  - **`imports`**: Lists imported components (e.g., `RouterOutlet`).
  - **`templateUrl`**: Points to the HTML template.
  - **`styleUrls`**: Specifies styles for the component.

**Alternate Property Definition:**

```typescript
private _title: string = 'miebay';
get title() { return this._title; }
set title(value: string) { this._title = value; }
```
- **Difference:** Using getters/setters provides more control over the property.

---

## **4. INTERACTION BETWEEN `index.html` AND COMPONENTS**

**`index.html`:**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Miebay</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

- **`<app-root>`**: Placeholder for `AppComponent`. Angular replaces this tag with the content defined in the `AppComponent` template.

---

## **5. DEPENDENCY INJECTION AND SERVICES**

- **What:** Mechanism to provide services throughout the app.
- **Example:** Services shared across components are instantiated once and reused.
- **Optimization:** Use a module to provide frequently used services and avoid repeated instantiation.

---

## **6. ROUTING IN ANGULAR**

**Routes Configuration:**

```typescript
import { Routes } from '@angular/router';
import { LoginComponent } from './components/zonaCliente/loginComponent/login.component';
import { RegistroComponent } from './components/zonaCliente/registroComponent/registro.component';

export const routes: Routes = [
  { path: 'Cliente/Login', component: LoginComponent },
  { path: 'Cliente/Registro', component: RegistroComponent }
];
```

- Each route object defines:
  - **`path`**: The URL segment (e.g., `'Cliente/Login'`).
  - **`component`**: The component to load.

**Important:**
- Routes should not start with `/`.
- Use `RouterOutlet` as a placeholder for routed components.

---

# QUESTIONS

>[!tip]- **What is the purpose of `main.ts`?**
>It bootstraps the Angular application by initializing the root component and application configuration.

>[!question]- **How does Angular know to replace `<app-root>` with the `AppComponent` template?**
>The `selector` property in the `@Component` decorator defines the tag name that Angular replaces with the component template.


>[!warning]- **What is the difference between defining a property directly and using getters/setters in Angular components?**
>Getters/setters provide additional control, like validation or computed values, while direct properties are simpler and faster for basic use cases.


>[!danger]- **How does `provideZoneChangeDetection` optimize Angular applications?**
>It coalesces DOM events to reduce the number of change detection cycles, improving performance by minimizing redundant checks.











src -> main.ts  el appcomponent, importa el componente que se carga al iniciar la app. app.component.ts

en el componente se exporta una clase :
export class AppComponent {
  title = 'miebay';
}

cuya propiedad prodia ser un atributo tipo:
	private _title:string='miebay'
	get title() { return this._title }
	set title (value:string) { this._title=value} 

funcionaria igual que lo de arriba. Explicame la diferencia en los apuntes y señala qué se suele usar.

Explica que es esta anotación que va antes del export class, es una funcion? y cada atributo:
@Component({
  selector: 'app-root',
  imports: [RouterOutlet, HeaderComponent,FooterComponent],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css',
})

Tambien en el templateUrl se puede meter codigo html directamente, eso es común? como funciona si importas componentes y exportas este componente? 

luego ese elector en el index.html aparece esto, el tag <app-root>, explica como funciona eso:
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Miebay</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>

qué es       <router-outlet /> ?? esta en un componente

el segundo argumento en main.ts  es appConfig.
import { bootstrapApplication } from '@angular/platform-browser';
import { appConfig } from './app/app.config';
import { AppComponent } from './app/components/zonaTienda/layoutComponents/app.component';

bootstrapApplication(AppComponent, appConfig)
  .catch((err) => console.error(err));

app.config.ts:

import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';

import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideZoneChangeDetection({ eventCoalescing: true }), provideRouter(routes)]
};
es un objeto definido en src/app/app.config.ts que implementa el interface AplicationConfig, define el comportamiento/configuracion de angular:
	- modulo de enrutamiento
	- modulo de inyeccion de dependencias
	- muchas mas cosas...

para evitar instanciar destruir instanciar destruir instanciar destruir, hay un modulo para aquellos servicios? objetos? componentes? que se usan muy frecuentemente. 

en mi app.component.html si quiero meter unos componentes como header y footer, tengo que coger el valor del selector de ese componente y meterlo en el html,  importarlo en app.component.ts no? arriba del todo y en improts. desarrollalo.

luego en cuanto a routing : 
import { Routes } from '@angular/router';
import { LoginComponent } from './components/zonaCliente/loginComponent/login.component';
import { RegistroComponent } from './components/zonaCliente/registroComponent/registro.component';
export const routes: Routes = [
  { path: 'Cliente/Login', component: LoginComponent},
  { path: 'Cliente/Registro', component: RegistroComponent}
];

cada elemento del objeto router es un objeto de la calse Route de angualr y representa el objeto encargado de decir al modulo de enroutamiento de angular qué componente carga ante una url en el navegador. Las rutas no pueden empezar con / no?
- - - 
