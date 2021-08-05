---
home: true
heroImage: /logo.svg
actionText: Comienzo
actionLink: /guide/

altActionText: Aprender más
altActionLink: /guide/why

features:
  - title: 💡 Inicio instantáneo del servidor
    details: Servicio de archivos bajo demanda a través de ESM nativo, ¡sin necesidad de bundling!
  - title: ⚡️ HMR rápido como un rayo
    details: Hot Module Replacement (HMR) que se mantiene rápido independientemente del tamaño de la app.
  - title: 🛠️ Ricas funcionalidades
    details: Soporte para TypeScript, JSX, CSS y más.
  - title: 📦 Build optimizado
    details: Compilación preconfigurada con Rollup, con soporte para el modo multipágina y modo biblioteca.
  - title: 🔩 Plugins universales
    details: Interfaz de plugins de Rollup-superset compartida entre dev y build.
  - title: 🔑 APIs totalmente tipadas
    details: APIs programáticas flexibles con tipado completo de TypeScript.
footer: Licencia MIT | Copyright © 2019-presente Evan You & Vite Contributors
---

<div class="frontpage sponsors">
  <h2>Patrocinadores</h2>
  <a v-for="{ href, src, name, id } of sponsors" :href="href" target="_blank" rel="noopener" aria-label="sponsor-img">
    <img :src="src" :alt="name" :id="`sponsor-${id}`">
  </a>
  <br>
  <a href="https://github.com/sponsors/yyx990803" target="_blank" rel="noopener">Ser patrocinador en GitHub</a>
</div>

<script setup>
import sponsors from './.vitepress/theme/sponsors.json'
</script>
