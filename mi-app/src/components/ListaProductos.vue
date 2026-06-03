<script setup>
import { ref, onMounted, onUpdated, onBeforeUnmount, useTemplateRef } from 'vue'
import TarjetaProducto from './TarjetaProducto.vue'

const props = defineProps({
  productos: {
    type: Array,
    required: true
  }
})

const box = useTemplateRef('box')
const listaFiltrada = ref([])
const cargando = ref(false)
let timer = null

const esperar = (ms) => {
  return new Promise(resolve => setTimeout(resolve, ms))
}

const cargarProductos = async () => {
  cargando.value = true
  await esperar(800)
  listaFiltrada.value = [...props.productos]
  cargando.value = false
}

onMounted(() => {
  cargarProductos()
  
  timer = setInterval(() => {
    console.log('Polling: Actualizando stock de productos...')
    cargarProductos()
  }, 30000)
})

onUpdated(() => {
  if (box.value) {
    box.value.scrollTop = box.value.scrollHeight
  }
})

onBeforeUnmount(() => {
  clearInterval(timer)
  console.log('ListaProductos desmontado - polling detenido')
})
</script>

<template>
  <div class="lista-container">
    
    <div v-if="cargando && listaFiltrada.length === 0" class="mensaje-carga">
      <span class="spinner">🌸</span> Cargando stock disponible...
    </div>

    <div v-else ref="box" class="lista-scrollable">
      <div v-for="(producto, index) in listaFiltrada" :key="producto.id" class="tarjeta-wrapper">
        
        <TarjetaProducto v-if="index === 0">
          <template #header>
            <h3 class="producto-nombre">{{ producto.nombre }}</h3>
            <span class="badge-categoria">{{ producto.categoria }}</span>
          </template>
          
          <template #body="{ expandida, toggleExpandir }">
            <p class="producto-precio">Precio: <strong>${{ producto.precio.toLocaleString() }}</strong></p>
            <button @click="toggleExpandir" class="btn-toggle">
              {{ expandida ? 'Ocultar info ▲' : 'Ver más info ▼' }}
            </button>
            <div v-if="expandida" class="info-extra">
              <p>Stock disponible: {{ producto.stock }} unidades</p>
            </div>
          </template>
          
          <template #footer>
            <button class="btn-accion primordial">Comprar Ahora</button>
          </template>
        </TarjetaProducto>

        <TarjetaProducto v-else-if="index === 1">
          <template #header>
            <h3 class="producto-nombre">{{ producto.nombre }}</h3>
            <span class="badge-categoria">{{ producto.categoria }}</span>
          </template>
          
          <template #body="{ expandida, toggleExpandir }">
            <p class="producto-precio">Precio: <strong>${{ producto.precio.toLocaleString() }}</strong></p>
            <button @click="toggleExpandir" class="btn-toggle">
              {{ expandida ? 'Ocultar info ▲' : 'Ver más info ▼' }}
            </button>
            <div v-if="expandida" class="info-extra">
              <p>Stock disponible: {{ producto.stock }} unidades</p>
            </div>
          </template>
        </TarjetaProducto>

        <TarjetaProducto v-else>
          <template #header>
            <h3 class="producto-nombre libre">{{ producto.nombre }}</h3>
            <span class="badge-categoria">{{ producto.categoria }}</span>
          </template>
          
          <template #body="{ expandida, toggleExpandir }">
            <div class="bloque-libre">
              <p class="precio-destacado">Oferta Especial: ${{ producto.precio.toLocaleString() }}</p>
            </div>
            <button @click="toggleExpandir" class="btn-toggle-libre">
              {{ expandida ? 'Menos detalles ▲' : 'Más detalles ▼' }}
            </button>
            <div v-if="expandida" class="info-extra-libre">
              <p>Contamos con {{ producto.stock }} artículos en bodega listos para despacho.</p>
            </div>
          </template>
          
          <template #footer>
            <span class="envio-gratis">🚚 Envío bonificado</span>
          </template>
        </TarjetaProducto>

      </div>
    </div>
  </div>
</template>

<style scoped>
.lista-container {
  width: 100%;
}
.lista-scrollable {
  display: flex;
  flex-direction: column;
  gap: 16px;
  max-height: 400px;
  overflow-y: auto;
  padding-right: 6px;
}

.mensaje-carga {
  text-align: center;
  padding: 40px;
  color: #ba9496;
  font-size: 1.1rem;
  font-style: italic;
  font-weight: 500;
}

.spinner {
  display: inline-block;
  animation: rotate 2s linear infinite;
}

@keyframes rotate {
  100% { transform: rotate(360deg); }
}

.tarjeta-wrapper {
  width: 100%;
}

.producto-nombre {
  margin: 0 0 4px 0;
  font-size: 1.15rem;
  color: #4a3b32;
}

.producto-nombre.libre {
  color: #6e5a4f;
  font-style: italic;
}

.badge-categoria {
  font-size: 0.75rem;
  background-color: #f6e8ea;
  color: #a65b61;
  padding: 2px 8px;
  border-radius: 20px;
  font-weight: bold;
}

.producto-precio {
  margin: 6px 0;
  color: #555;
}

.precio-destacado {
  margin: 6px 0;
  color: #a65b61;
  font-weight: bold;
}

.btn-toggle {
  background: none;
  border: none;
  color: #DBA9AB;
  cursor: pointer;
  padding: 0;
  font-size: 0.85rem;
  font-weight: bold;
  text-decoration: underline;
}

.btn-toggle-libre {
  background-color: #f5f5f5;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  padding: 4px 8px;
  cursor: pointer;
  font-size: 0.8rem;
  color: #555;
}

.info-extra {
  margin-top: 8px;
  padding: 8px;
  background-color: #fafafa;
  border-left: 3px solid #DBA9AB;
  font-size: 0.9rem;
}

.info-extra-libre {
  margin-top: 8px;
  padding: 8px;
  background-color: #fcf8f8;
  border-radius: 6px;
  font-size: 0.85rem;
  color: #777;
}

.btn-accion.primordial {
  background-color: #DBA9AB;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 0.85rem;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.2s;
}

.btn-accion.primordial:hover {
  background-color: #ebd3d5;
  color: #a65b61;
}

.tag-exclusivo {
  font-size: 0.7rem;
  background-color: #f0f0f0;
  color: #666;
  padding: 1px 6px;
  border-radius: 4px;
}
.envio-gratis {
  font-size: 0.85rem;
  color: #2b8a3e;
  font-weight: bold;
}
.lista-scrollable::-webkit-scrollbar {
  width: 5px;
}
.lista-scrollable::-webkit-scrollbar-track {
  background: #fdf8f8;
}
.lista-scrollable::-webkit-scrollbar-thumb {
  background: #ebd3d5;
  border-radius: 10px;
}
</style>