<script setup>
import { computed, onMounted, onUnmounted, onActivated, onDeactivated } from 'vue'
import ListaProductos from '../ListaProductos.vue'
import { productos } from '../../data/productos.js' // Importamos la base

const productosFiltrados = computed(() => {
  return productos.filter(p => p.categoria === 'Electrónica')
})

onMounted(() => console.log('TabElectronica - montado'))
onUnmounted(() => console.log('TabElectronica - desmontado'))
onActivated(() => console.log('TabElectronica - activado'))
onDeactivated(() => console.log('TabElectronica - desactivado'))
</script>

<template>
  <ListaProductos :productos="productosFiltrados" />
</template>