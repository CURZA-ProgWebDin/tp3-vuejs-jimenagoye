<script setup>
import { ref, onMounted, onUpdated, onBeforeUnmount, useTemplateRef } from 'vue'
import TarjetaProducto from './TarjetaProducto.vue'

// 1. Definición de las Props que pide el enunciado
const props = defineProps({
  productos: {
    type: Array,
    required: true
  }
})

// Estado de carga y variable para el intervalo del timer
const cargando = ref(true)
let timer = null

// 2. Referencia para el contenedor del div usando useTemplateRef
const box = useTemplateRef('box')

// Función auxiliar para simular el tiempo de red (Promise)
function esperar(ms) {
  return new Promise(resolve => setTimeout(resolve, ms))
}

// Función asíncrona para simular la carga de los productos
async function cargarProductos() {
  cargando.value = true
  await esperar(800) // Simula demora de red de 800ms
  cargando.value = false
}

// 3. Hook onMounted: Carga inicial y polling cada 30 segundos
onMounted(() => {
  cargarProductos()

  // Inicia el setInterval para simular actualización en segundo plano
  timer = setInterval(() => {
    console.log('Polling: Actualizando productos de forma asíncrona...')
    cargarProductos()
  }, 30000) // 30 segundos
})

// 4. Hook onUpdated: Corre el scroll automático al final de la lista
onUpdated(() => {
  if (box.value) {
    box.value.scrollTop = box.value.scrollHeight
  }
})

// 5. Hook onBeforeUnmount: Limpieza del intervalo para evitar fugas de memoria
onBeforeUnmount(() => {
  clearInterval(timer)
  console.log('ListaProductos desmontado – polling detenido')
})
</script>

<template>
  <div class="contenedor-lista">
    <div v-if="cargando" class="cargando-mensaje">
      <div class="spinner"></div>
      <p>Cargando productos...</p>
    </div>

    <div v-show="!cargando" ref="box" class="lista">
      <div class="grilla-productos">
        <TarjetaProducto 
          v-for="prod in props.productos" 
          :key="prod.id"
        >
          <template #header>
            <div class="header-producto">
              <h3>{{ prod.nombre }}</h3>
              <span class="badge-cat">{{ prod.categoria }}</span>
            </div>
          </template>

          <template #body="{ expandida, toggleExpandir }">
            <div class="body-producto">
              <p class="precio-txt">Precio: <strong>${{ prod.precio }}</strong></p>
              
              <div v-if="expandida" class="detalles-expandidos">
                <p><strong>Stock disponible:</strong> {{ prod.stock }} unidades</p>
                <p class="id-txt">ID Producto: #{{ prod.id }}</p>
              </div>

              <button @click="toggleExpandir" class="btn-toggle">
                {{ expandida ? 'Ocultar info 🔼' : 'Ver más info 🔽' }}
              </button>
            </div>
          </template>

          <template #footer>
            <div class="acciones-producto">
              <button class="btn-comprar">Agregar al carro</button>
            </div>
          </template>
        </TarjetaProducto>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Estilos del contenedor con scroll según pide el enunciado */
.lista {
  max-height: 450px;    /* Altura máxima obligatoria */
  overflow-y: auto;     /* Activa el scroll de forma automática */
  padding: 10px;
  background-color: #fbf5f6;
  border-radius: 8px;
  border: 1px solid #ebd3d5;
}

.grilla-productos {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

/* Diseño del mensaje de cargando */
.cargando-mensaje {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  color: #a65b61;
  font-weight: bold;
}

.spinner {
  border: 4px solid rgba(219, 169, 171, 0.2);
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border-left-color: #DBA9AB;
  animation: spin 1s linear infinite;
  margin-bottom: 10px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Detalles estéticos internos */
.header-producto {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-producto h3 {
  margin: 0;
  font-size: 1.1rem;
  color: #4a3b32;
}

.badge-cat {
  font-size: 0.75rem;
  background-color: #DBA9AB;
  color: white;
  padding: 3px 8px;
  border-radius: 12px;
}

.body-producto .precio-txt {
  margin: 5px 0;
  font-size: 1rem;
}

.detalles-expandidos {
  background-color: #ffffff;
  padding: 8px;
  border-radius: 6px;
  margin-top: 8px;
  border-left: 3px solid #DBA9AB;
  font-size: 0.9rem;
}

.id-txt {
  color: #999;
  font-size: 0.8rem;
  margin: 3px 0 0 0;
}

.btn-toggle {
  background: none;
  border: none;
  color: #a65b61;
  cursor: pointer;
  font-weight: bold;
  padding: 0;
  margin-top: 5px;
}

.btn-comprar {
  background-color: white;
  color: #a65b61;
  border: 1px solid #DBA9AB;
  padding: 6px 12px;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.2s;
}

.btn-comprar:hover {
  background-color: #DBA9AB;
  color: white;
}
</style>