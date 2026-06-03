<script setup>
import { onMounted, onUnmounted, onActivated, onDeactivated } from 'vue'
import ListaProductos from '../ListaProductos.vue'
import { productos } from '../../data/productos.js'

onMounted(() => console.log('TabTodos - montado'))
onUnmounted(() => console.log('TabTodos - desmontado'))
onActivated(() => console.log('TabTodos - activado'))
onDeactivated(() => console.log('TabTodos - desactivado'))
</script>

<template>
  <ListaProductos :productos="productos" />
</template>