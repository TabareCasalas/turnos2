<!-- components/SelectorProduccion.vue -->
<template>
    <div class="flex flex-col items-center justify-center min-h-screen text-center">
      <h1 class="text-2xl font-bold mb-6">Seleccionar Producción</h1>
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
        <button
          v-for="n in 4"
          :key="n"
          @click="$emit('seleccionar', n)"
          class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-6 rounded-xl shadow-md transition"
        >
          Producción {{ n }}
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'SelectorProduccion'
  }
  </script>
  