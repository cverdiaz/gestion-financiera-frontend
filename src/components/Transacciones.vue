<template>
    <div>
      <h2>Lista de Transacciones</h2>
  
      <div v-if="transacciones.length === 0" class="alert alert-info">
        No hay transacciones registradas.
      </div>
  
      <ul class="list-group">
  <li
    v-for="transaccion in transacciones"
    :key="transaccion.id"
    class="list-group-item d-flex justify-content-between align-items-center"
  >
    <div>
      <strong>{{ transaccion.tipo.toUpperCase() }}</strong> - ${{ transaccion.monto }} - {{ transaccion.descripcion }} - {{ transaccion.fecha }}
    </div>
    <div>
      <button @click="editarTransaccion(transaccion)" class="btn btn-sm btn-warning me-2">Editar</button>
      <button @click="eliminarTransaccion(transaccion.id)" class="btn btn-sm btn-danger">Eliminar</button>
    </div>
  </li>
</ul>
    </div>
  </template>
  
  <script setup>
  import api from '@/services/api'
  import { defineProps, defineEmits } from 'vue'
  import { useToast } from 'vue-toastification'

  const toast = useToast()

  const emit = defineEmits(['editar-transaccion'])
  
  const props = defineProps({
    transacciones: {
      type: Array,
      required: true
    }
  })
  
  // Función auxiliar para mostrar fecha en formato local
  const formatearFecha = (fecha) => {
    return new Date(fecha).toLocaleDateString('es-CL', {
      day: '2-digit',
      month: 'short',
      year: 'numeric'
    })
  }

  const eliminarTransaccion = async (id) => {
    if (!confirm('¿Estás seguro de que deseas eliminar esta transacción?')) return;

    try {
      await api.delete(`/transacciones/${id}`)
      toast.success('Transacción eliminada')
      emit('editar-transaccion') // para que el padre vuelva a cargar la lista
    } catch (error) {
      toast.error('Error al eliminar transacción')
      console.error(error)
    }
  }

  const editarTransaccion = (transaccion) => {
    emit('editar-transaccion', transaccion)
    console.log('Editar transacción:', transaccion)
  }
  </script>
  