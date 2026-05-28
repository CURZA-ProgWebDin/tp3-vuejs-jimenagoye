<script setup>
/* RESPUESTA ENUNCIADO: ¿En qué situación conviene usar KeepAlive y en cuál no?
  1. CUÁNDO CONVIENE: Conviene cuando el componente de la pestaña realiza operaciones costosas
     (como llamadas asíncronas, filtrados pesados o cargas de red) o cuando el usuario modifica
     el estado interno (como abrir el "Ver más info" de una tarjeta o mover el scroll). 
     Al usar KeepAlive, cuando el usuario cambia de pestaña y vuelve, todo sigue exactamente
     en su lugar sin recargar ni perder la posición.
     
  2. CUÁNDO NO CONVIENE: No conviene si el negocio exige que los datos estén 100% actualizados
     en tiempo real cada vez que se hace clic en la pestaña, o si el sitio cuenta con decenas 
     de pestañas extremadamente masivas, ya que KeepAlive retiene los componentes vivos en la 
     memoria RAM del navegador y podría consumir recursos innecesarios.*/

import { ref, computed } from 'vue'
import TabTodos from './components/tabs/TabTodos.vue'
import TabElectronica from './components/tabs/TabElectronica.vue'
import TabPerifericos from './components/tabs/TabPerifericos.vue'

// Usamos un diccionario de componentes como explica Eduardo en clase
const listaComponentes = {
  Todos: TabTodos,
  Electronica: TabElectronica,
  Perifericos: TabPerifericos
}

const tabActual = ref('Todos')

const componenteActivo = computed(() => {
  return listaComponentes[tabActual.value]
})
</script>

<template>
  <div class="panel-pestanas">
    
    <div class="botones-container">
      <button :class="{ activo: tabActual === 'Todos' }" @click="tabActual = 'Todos'">
        Todos
      </button>
      <button :class="{ activo: tabActual === 'Electronica' }" @click="tabActual = 'Electronica'">
        Electrónica
      </button>
      <button :class="{ activo: tabActual === 'Perifericos' }" @click="tabActual = 'Perifericos'">
        Periféricos
      </button>
    </div>

    <div class="comparativa-grilla">
      <div class="columna-ejemplo">
        <h2 class="titulo-ejemplo">Estado Normal (Sin KeepAlive)</h2>
        <div class="caja-render">
          <component :is="componenteActivo" />
        </div>
      </div>

      <div class="columna-ejemplo">
        <h2 class="titulo-ejemplo">Estado Optimizado (Con KeepAlive)</h2>
        <div class="caja-render">
          <KeepAlive>
            <component :is="componenteActivo" />
          </KeepAlive>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.panel-pestanas {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.botones-container {
  display: flex;
  justify-content: center;
  gap: 12px;
  background-color: white;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.02);
}

button {
  background-color: #f5f5f5;
  border: 1px solid #e0e0e0;
  padding: 10px 20px;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  color: #555;
  transition: all 0.2s ease;
}

button:hover {
  background-color: #ebd3d5;
}

button.activo {
  background-color: #DBA9AB;
  color: white;
  border-color: #a65b61;
}

.comparativa-grilla {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

@media (max-width: 768px) {
  .comparativa-grilla {
    grid-template-columns: 1fr;
  }
}

.columna-ejemplo {
  background-color: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.04);
}

.titulo-ejemplo {
  font-size: 1.1rem;
  color: #4a3b32;
  border-bottom: 2px solid #f6e8ea;
  padding-bottom: 10px;
  margin-top: 0;
}

.caja-render {
  margin-top: 15px;
}
</style>