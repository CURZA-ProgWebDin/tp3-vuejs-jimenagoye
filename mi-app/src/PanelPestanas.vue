<script setup>
import { ref } from 'vue'
import TabTodos from './components/tabs/TabTodos.vue'
import TabElectronica from './components/tabs/TabElectronica.vue'
import TabPerifericos from './components/tabs/TabPerifericos.vue'

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
      <button 
        v-for="(_, nombre) in pestañas" 
        :key="nombre"
        :class="['btn-pestana', { activo: tabActivo === nombre }]"
        @click="tabActivo = nombre"
      >
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

    </div>

  </div>
</template>

<style scoped>
.panel-container {
  width: 100%;
  max-width: 1000px;
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
  gap: 24px;
}

.columna {
  flex: 1;
  background-color: #ffffff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(166, 91, 97, 0.04);
  border: 1px solid #fcf8f8;
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
  max-height: 450px;
  overflow-y: auto;
  padding-right: 5px;
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

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>