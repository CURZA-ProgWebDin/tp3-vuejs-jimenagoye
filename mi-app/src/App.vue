<script setup>
import PanelPestanas from './PanelPestanas.vue'
</script>

<template>
  <main class="contenedor-principal">
    <header class="app-header">
      <h1>Catálogo de Productos Avanzado</h1>
      <p class="subtitulo">TP3 — Ciclo de Vida, Slots y Componentes Dinámicos</p>
    </header>

    <PanelPestanas />
  </main>
</template>

<style>
body {
  background-color: #f6e8ea; /* Tu rosa pastel característico */
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 0;
  padding: 20px;
  color: #333;
}

.contenedor-principal {
  max-width: 1100px;
  margin: 0 auto;
}

.app-header {
  text-align: center;
  margin-bottom: 30px;
}

.app-header h1 {
  color: #4a3b32;
  margin-bottom: 5px;
}

.subtitulo {
  color: #8c766b;
  margin: 0;
  font-size: 0.95rem;
}
</style>