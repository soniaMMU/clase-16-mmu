<template>
  <div>
    <div style="text-align: center; margin-top: 10px;">
      <label style="font-weight: bold; font-size: 1.2rem;">Selecciona una figura: </label>
      <select v-model="figuraSeleccionada" style="margin: 10px; padding: 5px; font-size: 1rem;">
        <option value="cubo">Cubo</option>
        <option value="hexagono">Hexágono</option>
      </select>
    </div>
    <component :is="figuraSeleccionadaComponente" />
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue'
import Cubo from './components/Cubo.vue'
import Hexagono from './components/Hexagono.vue'

const figuraSeleccionada = ref('cubo')

const figuraSeleccionadaComponente = computed(() => {
  return figuraSeleccionada.value === 'cubo' ? Cubo : Hexagono
})
</script>
