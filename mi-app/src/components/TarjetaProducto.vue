<script setup>
import { ref } from 'vue'

const expandida = ref(false)
const toggleExpandir = () => {
  expandida.value = !expandida.value
}
</script>

<template>
  <div class="tarjeta-producto">
    <header class="tarjeta-header">
      <slot name="header"></slot>
    </header>

    <main class="tarjeta-body">
      <slot name="body" :expandida="expandida" :toggleExpandir="toggleExpandir"></slot>
    </main>

    <footer class="tarjeta-footer">
      <slot name="footer">
        <span class="sin-acciones">Sin acciones disponibles</span>
      </slot>
    </footer>
  </div>
</template>

<style scoped>
.tarjeta-producto {
  background-color: white;
  border-radius: 12px;
  padding: 15px;
  margin-bottom: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
  border: 1px solid #f0f0f0;
}
.tarjeta-header { font-weight: bold; margin-bottom: 8px; }
.tarjeta-body { margin-bottom: 10px; color: #555; }
.tarjeta-footer { border-top: 1px solid #f9f9f9; padding-top: 8px; }
.sin-acciones { color: #999; font-style: italic; font-size: 0.9rem; }
</style>