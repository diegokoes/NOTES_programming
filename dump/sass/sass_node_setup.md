# SASS -> Set-up -> 
## Configuración de Sass en un Proyecto Vite

Este documento proporciona una guía para configurar Sass en un proyecto Vite, asumiendo que ya tienes instalados [[nodejs]] y [[nodejs_npm]].

## Creación del Proyecto con Vite

**Nuevo Proyecto**

**1. Crear un nuevo proyecto con [[Vite]]:**

``` bash
						npm create vite@latest ejemploSass
```

**Navegar al directorio del proyecto:**
```bash
						cd ejemploSass
```
**Instalar las dependencias:**
```bash
						npm install
```

 **Opción B: Proyecto Existente**

1. **Navegar al directorio del proyecto existente.**
    
2. **Instalar Vite como dependencia de desarrollo:**
``` bash
						npm install vite --save-dev
```

## Instalación de Sass

1. **Instalar Sass como dependencia de desarrollo:**
```bash
						npm install sass --save-dev
```

## Ejecución del Proyecto

2. **Iniciar el servidor de desarrollo de Vite:**
```bash
						npm run dev  
```

Este comando inicia el servidor de desarrollo y aplica Hot Module Replacement (HMR) para reflejar inmediatamente los cambios en tus archivos `.scss` en el navegador sin necesidad de recargar la página.

## Generar un directorio `dist` de sass->css

```bash
						sass --watch src/styles:dist/styles
```