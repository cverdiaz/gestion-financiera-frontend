<template>
  <div class="container">
    <h1 class="mb-4">Gestión Financiera</h1>
    <AgregarTransaccion 
     :transaccionEditar="transaccionSeleccionada"
      @transaccion-creada="obtenerTransacciones" 
    />
    <hr />
    <Transacciones 
      :transacciones="transacciones"  
      @editar-transaccion="transaccionSeleccionada = $event" 
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import api from './services/api'
import Transacciones from './components/Transacciones.vue'
import AgregarTransaccion from './components/AgregarTransaccion.vue'

const transacciones = ref([])
const transaccionSeleccionada = ref(null)

const obtenerTransacciones = async () => {
  try {
    const response = await api.get('/transacciones')
    transacciones.value = response.data
    transaccionSeleccionada.value = null // Reinicia edición
  } catch (error) {
    console.error('Error al obtener transacciones:', error)
  }
}

onMounted(() => {
  obtenerTransacciones()
})

// ✅ Esta función es llamada cuando se emite 'transaccion-agregada'
const agregarTransaccion = (nueva) => {
  transacciones.value.unshift(nueva)
}
</script>