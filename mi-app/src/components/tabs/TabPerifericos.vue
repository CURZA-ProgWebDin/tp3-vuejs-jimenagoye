<script setup>
import { computed, onMounted, onUnmounted, onActivated, onDeactivated } from 'vue'
import { productos } from '../../data/productos.js'
import ListaProductos from '../ListaProductos.vue'

onMounted(() => console.log('TabPerifericos – montado'))
onUnmounted(() => console.log('TabPerifericos – desmontado'))
onActivated(() => console.log('TabPerifericos – activado'))
onDeactivated(() => console.log('TabPerifericos – desactivado'))

const productosFiltrados = computed(() => {
  return productos.filter(p => p.categoria === 'Periféricos')
})
</script>

<template>
  <div class="tab-contenido">
    <h3 class="titulo-categoria">Mostrando productos de Periféricos</h3>
    <ListaProductos :productos="productosFiltrados" />
  </div>
</template>

<style scoped>
.tab-contenido {
  animation: fadeIn 0.3s ease;
}

.titulo-categoria {
  color: #4a3b32;
  font-size: 1.1rem;
  margin-bottom: 15px;
  text-align: center;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(5px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>