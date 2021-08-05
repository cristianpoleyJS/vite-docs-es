---
sidebar: false
---

# Announcing Vite 2.0

<p style="text-align:center">
  <img src="/logo.svg" style="height:200px">
</p>

¡Hoy nos complace anunciar el lanzamiento oficial de Vite 2.0!

Vite (palabra francesa para "rápido", pronunciada `/vit/`) es una nueva herramienta de construcción de frontales. Piénsalo como unn servidor de desarrollo + constructor preconfigurado, pero más ligero y rápido. Aprovecha el soporte de [native ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) de los navegadores y las herramientas escritas en lenguajes de compilación nativos como [esbuild](https://esbuild.github.io/) para ofrecer una experiencia de desarrollo ágil y moderna.

Para tener una idea de lo rápido que es Vite, echa un vistazo a [esta comparación en vídeo](https://twitter.com/amasad/status/1355379680275128321) del arranque de una aplicación React en Repl.it usando Vite frente a `create-react-app` (CRA).

Si nunca has oído hablar de Vite y te gustaría saber más sobre él, echa un vistazo a [la razón de ser del proyecto](https://vitejs.dev/guide/why.html). Si te interesa saber en qué se diferencia Vite de otras herramientas similares, echa un vistazo a las [comparaciones](https://vitejs.dev/guide/comparisons.html).

## Novedades de la versión 2.0

Dado que decidimos refactorizar por completo las funciones internas antes de que la versión 1.0 saliera de la RC, ésta es de hecho la primera versión estable de Vite. Dicho esto, Vite 2.0 trae muchas mejoras sobre la versión anterior:

### Core del Framework Agnóstico

La idea original de Vite comenzó como un [prototipo hacky que sirve componentes Vue de un solo archivo sobre ESM nativo](https://github.com/vuejs/vue-dev-server). Vite 1.0 era una continuación de esa idea con un HMR implementado encima.

Vite 2.0 toma lo que hemos aprendido en el camino y se rediseña desde cero con una arquitectura interna más robusta. Ahora es completamente agnóstica al framework, y todo el soporte específico al framework se delega a los plugins. Ahora hay [plantillas oficiales para Vue, React, Preact, Svelte, Lit Element](https://github.com/vitejs/vite/tree/main/packages/create-vite).

### Nuevo formato y API de los plugins

Inspirado en [WMR](https://github.com/preactjs/wmr), el nuevo sistema de plugins amplía la interfaz de plugins de Rollup y es [compatible con muchos plugins de Rollup](https://vite-rollup-plugins.patak.dev/) desde el principio. Los plugins pueden utilizar hooks compatibles con Rollup, con hooks y propiedades adicionales específicos de Vite para ajustar el comportamiento exclusivo de Vite (por ejemplo, diferenciando dev vs. build o el manejo personalizado de HMR).

La [API programática](https://vitejs.dev/guide/api-javascript.html) también ha sido mejorada en gran medida para facilitar herramientas / frameworks de mayor nivel construidos sobre Vite.

### esbuild Powered Dep Pre-Bundling

Dado que Vite es un servidor de desarrollo nativo de ESM, pre-agrupa las dependencias para reducir el número de peticiones del navegador y manejar la conversión de CommonJS a ESM. Anteriormente Vite hacía esto usando Rollup, y en la versión 2.0 ahora usa `esbuild` que resulta en una pre-agrupación de dependencias 10-100 veces más rápida. Como referencia, el arranque en frío de una aplicación de prueba con dependencias pesadas como React Material UI antes tardaba 28 segundos en un Macbook Pro con M1 y ahora tarda ~1,5 segundos. Espere mejoras similares si usted está cambiando de una configuración tradicional basada en bundler.

### Soporte de CSS de primera clase

Vite trata el CSS como un ciudadano de primera clase del gráfico del módulo y soporta lo siguiente fuera de la caja:

- **Mejora de la resolución**: Las rutas `@import` y `url()` en CSS son mejoradas con el resolvedor de Vite para respetar los alias y las dependencias de npm.
- Reestructuración de URLs**: Las rutas de `url()` se reajustan automáticamente sin importar de dónde se importe el archivo.
- **División de código CSS**: un trozo de JS dividido en código también emite un archivo CSS correspondiente, que se carga automáticamente en paralelo con el trozo de JS cuando se solicita.

### Soporte de renderizado del lado del servidor (SSR)

Vite 2.0 viene con [soporte experimental de SSR](https://vitejs.dev/guide/ssr.html). Vite proporciona APIs para cargar y actualizar eficientemente el código fuente basado en ESM en Node.js durante el desarrollo (casi como HMR del lado del servidor), y externaliza automáticamente las dependencias compatibles con CommonJS para mejorar el desarrollo y la velocidad de construcción de SSR. El servidor de producción puede ser completamente desacoplado de Vite, y la misma configuración puede ser fácilmente adaptada para realizar el pre-renderizado / SSG.

Vite SSR se proporciona como una funcionalidad de bajo nivel y esperamos ver frameworks que lo utilicen.

### Soporte para navegadores antiguos

Vite se dirige a los navegadores modernos con soporte nativo de ESM por defecto, pero también se puede optar por el soporte de los navegadores legacy a través del plugin oficial [@vitejs/plugin-legacy](https://github.com/vitejs/vite/tree/main/packages/plugin-legacy). El plugin genera automáticamente paquetes modernos/legacy, y entrega el paquete correcto basado en la detección de funcionalidades del navegador, asegurando un código más eficiente en los navegadores modernos que los soportan.

## ¡Pruébalo!

Son muchas funcionalidades, pero empezar a usar Vite es muy sencillo. Puedes poner en marcha una aplicación generada por Vite literalmente en un minuto, lanzando el siguiente comando (asegúrate de que tienes Node.js >=12):

```bash
npm init @vitejs/app
```

A continuación, echa un vistazo a [la guía](https://es.vitejs.dev/guide/) para ver lo que Vite proporciona "fuera de la caja". También puedes consultar el código fuente en [GitHub](https://github.com/vitejs/vite), seguir las actualizaciones en [Twitter](https://twitter.com/vite_js), o unirte a las discusiones con otros usuarios de Vite en nuestro [servidor de Discord](http://chat.vitejs.dev/).
