<template>
  <div class="min-h-screen bg-gray-100 p-4">
    <div class="text-center mb-4">
      <button
        v-if="produccion"
        @click="resetProduccion"
        class="mt-4 px-4 py-2 bg-gray-800 text-white rounded hover:bg-gray-700"
      >
        Producción {{ produccion }}
      </button>
    </div>

    <SelectorProduccion v-if="!produccion" @seleccionar="guardarProduccion" />
    <CalendarioTurnos v-else :produccion="produccion" />
  </div>
</template>

<script>
import SelectorProduccion from './components/SelectorProduccion.vue'
import CalendarioTurnos from './components/CalendarioTurnos.vue'

export default {
  name: 'App',
  components: {
    SelectorProduccion,
    CalendarioTurnos
  },
  data() {
    return {
      produccion: localStorage.getItem('produccion') || null
    }
  },
  methods: {
    guardarProduccion(valor) {
      this.produccion = valor
      localStorage.setItem('produccion', valor)
    },
    resetProduccion() {
      localStorage.removeItem('produccion')
      this.produccion = null
    }
  }
}
</script>

