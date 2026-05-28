<script setup>
import { computed, onMounted, onUnmounted, onActivated, onDeactivated } from 'vue'
import { productos } from '../../data/productos.js'
import ListaProductos from '../ListaProductos.vue'

// HOOKS DE CICLO DE VIDA OBLIGATORIOS POR LA GUÍA
onMounted(() => console.log('TabElectronica – montado'))
onUnmounted(() => console.log('TabElectronica – desmontado'))
onActivated(() => console.log('TabElectronica – activado'))
onDeactivated(() => console.log('TabElectronica – desactivado'))

// FILTRADO: Nos aseguramos de buscar exactamente "Electrónica" con tilde y mayúscula
const productosFiltrados = computed(() => {
  return productos.filter(p => p.categoria === 'Electrónica')
})
</script>

<template>
  <div class="tab-contenido">
    <h3 class="titulo-categoria">Mostrando productos de Electrónica</h3>
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