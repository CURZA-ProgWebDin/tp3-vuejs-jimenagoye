<script setup>
import { computed, onMounted, onUnmounted, onActivated, onDeactivated } from 'vue'
import ListaProductos from '../ListaProductos.vue'
import { productos } from '../../data/productos.js'

const productosFiltrados = computed(() => {
  return productos.filter(p => p.categoria === 'Periféricos')
})

onMounted(() => console.log('TabPerifericos - montado'))
onUnmounted(() => console.log('TabPerifericos - desmontado'))
onActivated(() => console.log('TabPerifericos - activado'))
onDeactivated(() => console.log('TabPerifericos - desactivado'))
</script>

<template>
  <ListaProductos :productos="productosFiltrados" />
</template>