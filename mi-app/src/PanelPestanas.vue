<script setup>
import { ref } from 'vue'
import TabTodos from './components/tabs/TabTodos.vue'
import TabElectronica from './components/tabs/TabElectronica.vue'
import TabPerifericos from './components/tabs/TabPerifericos.vue'
import TarjetaProducto from './components/TarjetaProducto.vue'
import { productos } from './data/productos.js'

const prods = productos

const pestañas = {
  Todos: TabTodos,
  Electronica: TabElectronica,
  Perifericos: TabPerifericos
}

const tabActivo = ref('Todos')
const slugText = (text) => {
  return text
    .toString()
    .split(/(?=[A-Z])/)
    .join(" ")
}
</script>

<template>
  <div class="panel-container">

    <nav class="barra-pestanas">
      <button v-for="(_, nombre) in pestañas" :key="nombre" :class="['btn-pestana', { activo: tabActivo === nombre }]"
        @click="tabActivo = nombre">
        {{ nombre === 'Electronica' ? 'Electrónica' : nombre === 'Perifericos' ? 'Periféricos' : slugText(nombre) }}
      </button>
    </nav>

    <div class="columnas-layout">
      
      <div class="columna">
        <h3>Estado Normal (Sin KeepAlive)</h3>
        <div class="caja-render">
          <Transition name="fade" mode="out-in">
            <component :is="pestañas[tabActivo]" />
          </Transition>
        </div>
      </div>
      
      <div class="columna">
        <h3>Estado Optimizado (Con KeepAlive)</h3>
        <div class="caja-render">
          <Transition name="fade" mode="out-in">
            <KeepAlive>
              <component :is="pestañas[tabActivo]" />
            </KeepAlive>
          </Transition>
        </div>
      </div>

      <div class="columna">
        <h3>Demostración slots (Parte 1)</h3>
        <div class="caja-render instancias-fijas">
          
          <div class="bloque-instancia">
            <span class="label-instancia"></span>
            <TarjetaProducto>
              <template #header>
                <h3 class="producto-nombre">{{ prods[0].nombre }}</h3>
                <span class="badge-categoria">{{ prods[0].categoria }}</span>
              </template>
              
              <template #body="{ expandida, toggleExpandir }">
                <p class="producto-precio">Precio: <strong>${{ prods[0].precio.toLocaleString() }}</strong></p>
                <button @click="toggleExpandir" class="btn-toggle">
                  {{ expandida ? 'Ocultar info ▲' : 'Ver más info ▼' }}
                </button>
                <div v-if="expandida" class="info-extra">
                  <p>Stock disponible: {{ prods[0].stock }} unidades</p>
                </div>
              </template>
              
              <template #footer>
                <button class="btn-accion primordial">Comprar Ahora</button>
              </template>
            </TarjetaProducto>
          </div>
          
          <div class="bloque-instancia">
            <span class="label-instancia"></span>
            <TarjetaProducto>
              <template #header>
                <h3 class="producto-nombre">{{ prods[1].nombre }}</h3>
                <span class="badge-categoria">{{ prods[1].categoria }}</span>
              </template>
              
              <template #body="{ expandida, toggleExpandir }">
                <p class="producto-precio">Precio: <strong>${{ prods[1].precio.toLocaleString() }}</strong></p>
                <button @click="toggleExpandir" class="btn-toggle">
                  {{ expandida ? 'Ocultar info ▲' : 'Ver más info ▼' }}
                </button>
                <div v-if="expandida" class="info-extra">
                  <p>Stock disponible: {{ prods[1].stock }} unidades</p>
                </div>
              </template>
            </TarjetaProducto>
          </div>

          <div class="bloque-instancia">
            <span class="label-instancia"></span>
            <TarjetaProducto>
              <template #header>
                <h3 class="producto-nombre libre">{{ prods[2].nombre }}</h3>
                <span class="badge-categoria">{{ prods[2].categoria }}</span>
              </template>
              
              <template #body="{ expandida, toggleExpandir }">
                <div class="bloque-libre">
                  <p class="precio-destacado">Oferta Especial: ${{ prods[2].precio.toLocaleString() }}</p>
                </div>
                <button @click="toggleExpandir" class="btn-toggle-libre">
                  {{ expandida ? 'Menos detalles ▲' : 'Más detalles ▼' }}
                </button>
                <div v-if="expandida" class="info-extra-libre">
                  <p>Contamos con {{ prods[2].stock }} artículos en bodega listos para despacho.</p>
                </div>
              </template>
              
              <template #footer>
                <span class="envio-gratis">🚚 Envío bonificado</span>
              </template>
            </TarjetaProducto>
          </div>

        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>
.panel-container {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.barra-pestanas {
  display: flex;
  justify-content: center;
  gap: 12px;
  background-color: #ffffff;
  padding: 14px;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.02);
  margin-bottom: 20px;
}

.btn-pestana {
  background-color: #f5f5f5;
  border: 1px solid #eef0f2;
  color: #555;
  padding: 8px 16px;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s ease;
}

.btn-pestana:hover {
  background-color: #ebd3d5;
  color: #a65b61;
}

.btn-pestana.activo {
  background-color: #DBA9AB;
  color: white;
  border-color: #DBA9AB;
}

.columnas-layout {
  display: flex;
  gap: 20px;
}

.columna {
  flex: 1;
  background-color: #ffffff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(166, 91, 97, 0.04);
  border: 1px solid #fcf8f8;
  min-width: 0;
}

.columna h3 {
  text-align: center;
  font-size: 1.1rem;
  color: #4a3b32;
  margin-top: 0;
  margin-bottom: 15px;
  border-bottom: 2px solid #f6e8ea;
  padding-bottom: 10px;
}

.caja-render {
  margin-top: 15px;
  max-height: 500px;
  overflow-y: auto;
  padding-right: 5px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.instancias-fijas {
  gap: 24px;
}

.bloque-instancia {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.label-instancia {
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ba9496;
  font-weight: bold;
}
.caja-render::-webkit-scrollbar {
  width: 6px;
}
.caja-render::-webkit-scrollbar-track {
  background: #f5f5f5;
  border-radius: 4px;
}
.caja-render::-webkit-scrollbar-thumb {
  background: #DBA9AB;
  border-radius: 4px;
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
.envio-gratis {
  font-size: 0.85rem;
  color: #2b8a3e;
  font-weight: bold;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>