<script setup>
import { onMounted, onUnmounted, onActivated, onDeactivated } from 'vue'
import { productos } from '../../data/productos'
import ListaProductos from '../ListaProductos.vue'

onMounted(() => console.log('TabTodos – montado'))
onUnmounted(() => console.log('TabTodos – desmontado'))
onActivated(() => console.log('TabTodos – activado'))
onDeactivated(() => console.log('TabTodos – desactivado'))
</script>

<template>
  <div class="tab-contenido">
    <h3>Mostrando Todos los Productos</h3>
    <ListaProductos :productos="productos" />
  </div>
</template>