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
      <slot name="header" />
    </header>

    <main class="tarjeta-body">
      <slot 
        name="body" 
        :expandida="expandida" 
        :toggleExpandir="toggleExpandir" 
      />
    </main>

    <footer class="tarjeta-footer">
      <slot name="footer">
        <span class="fallback-texto">Sin acciones disponibles</span>
      </slot>
    </footer>
  </div>
</template>

<style scoped>
.tarjeta-producto {
  background-color: #ffffff;
  border: 1px solid #f6e8ea;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 14px;
  box-shadow: 0 4px 12px rgba(166, 91, 97, 0.04);
}

.tarjeta-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  border-bottom: 1px solid #fdf8f8;
  padding-bottom: 6px;
}

.tarjeta-body {
  padding: 6px 0;
  font-size: 0.95rem;
}

.tarjeta-footer {
  margin-top: 14px;
  padding-top: 10px;
  border-top: 1px dashed #f6e8ea;
  display: flex;
  justify-content: flex-end;
}

.fallback-texto {
  font-size: 0.85rem;
  color: #ba9496;
  font-style: italic;
}
</style>