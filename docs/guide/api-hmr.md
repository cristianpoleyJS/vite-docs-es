# HMR API

:::tip Nota
Esta es la API HMR del cliente. Para manejar la actualización de HRM en los plugins, visita [handleHotUpdate](./api-plugin#handlehotupdate).

La API manual de HMR está destinada principalmente a los autores de frameworks y herramientas. Como usuario final, es probable que HMR ya se maneje en las plantillas de inicio del framework.
:::

Vite expone su API HMR a través del objeto especial `import.meta.hot`:

```ts
interface ImportMeta {
  readonly hot?: {
    readonly data: any

    accept(): void
    accept(cb: (mod: any) => void): void
    accept(dep: string, cb: (mod: any) => void): void
    accept(deps: string[], cb: (mods: any[]) => void): void

    dispose(cb: (data: any) => void): void
    decline(): void
    invalidate(): void

    on(event: string, cb: (...args: any[]) => void): void
  }
}
```

## Bloque condicional necesario

En primer lugar, asegúrate de proteger todo el uso de la API de HMR con un bloque condicional para que se pueda aplicar el tree-shaking al código de producción:

```js
if (import.meta.hot) {
  // HMR code
}
```

## `hot.accept(cb)`

Para que un módulo se autoacepte, utiliza `import.meta.hot.accept` con un callback que reciba el módulo actualizado:

```js
export const count = 1

if (import.meta.hot) {
  import.meta.hot.accept((newModule) => {
    console.log('updated: count is now ', newModule.count)
  })
}
```
Un módulo que "acepta" actualizaciones en "caliente" se considera un **Límite HMR**.

Ten en cuenta que el HMR de Vite no intercambia el módulo original importado: si un módulo boundary HMR reexporta importaciones de un dep, entonces es responsable de actualizar esas reexportaciones (y estas exportaciones deben usar `let`). Además, los importadores de la cadena del módulo boundary no serán notificados del cambio.

Esta implementación simplificada de HMR es suficiente para la mayoría de los casos de uso de los desarrolladores, al tiempo que nos permite omitir el costoso trabajo de generar módulos proxy.

## `hot.accept(deps, cb)`

Un módulo también puede aceptar actualizaciones de dependencias directas sin recargarse a sí mismo:

```js
import { foo } from './foo.js'

foo()

if (import.meta.hot) {
  import.meta.hot.accept('./foo.js', (newFoo) => {
    // el callback recibe el módulo actualizado './foo.js'
    newFoo.foo()
  })

  // También puede aceptar un array de módulos dep:
  import.meta.hot.accept(
    ['./foo.js', './bar.js'],
    ([newFooModule, newBarModule]) => {
      // el callback recibe los módulos actualizados en un Array
    }
  )
}
```

## `hot.dispose(cb)`

Un módulo que se acepta a sí mismo o un módulo que espera ser aceptado por otros puede utilizar `hot.dispose` para limpiar cualquier efecto secundario persistente creado por su copia actualizada:

```js
function setupSideEffect() {}

setupSideEffect()

if (import.meta.hot) {
  import.meta.hot.dispose((data) => {
    // elimina los efectos secundarios
  })
}
```

## `hot.data`

El objeto `import.meta.hot.data` es persistido a través de diferentes instancias del mismo módulo actualizado. Se puede utilizar para pasar información de una versión anterior del módulo a la siguiente.

## `hot.decline()`

Llama a `import.meta.hot.decline()` para indicar que este módulo no es actualizable en caliente, y el navegador debe realizar una recarga completa si se encuentra este módulo mientras se propagan las actualizaciones HMR.

## `hot.invalidate()`

Por ahora, llamar a `import.meta.hot.invalidate()` simplemente recarga la página.

## `hot.on(evento, cb)`

Escucha un evento HMR.

Los siguientes eventos HMR son enviados por Vite automáticamente:

- `'vite:beforeUpdate'` cuando una actualización está a punto de ser aplicada (por ejemplo, un módulo será reemplazado)
- `'vite:beforeFullReload'` cuando se va a producir una recarga completa
- `'vite:beforePrune'` cuando los módulos que ya no son necesarios están a punto de ser pasados por el tree-shaking
- `'vite:error'` cuando se produce un error (por ejemplo, de sintáxis)

También se pueden enviar eventos HMR personalizados desde los plugins. Ver [handleHotUpdate](./api-plugin#handlehotupdate) para más detalles.
