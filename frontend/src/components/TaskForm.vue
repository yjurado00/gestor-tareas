<script setup>
import { ref } from 'vue'
const emit = defineEmits(['task-created'])
const title = ref('')
const description = ref('')
const error = ref('')

async function submit() {
  if (!title.value.trim()) { error.value = 'El título es obligatorio'; return }
  try {
    const res = await fetch('http://localhost:8000/api/tasks', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ title: title.value, description: description.value })
    })
    if (!res.ok) throw new Error('Error al crear')
    const task = await res.json()
    emit('task-created', task)
    title.value = ''; description.value = ''; error.value = ''
  } catch (e) { error.value = e.message }
}
</script>

<template>
  <div class="form">
    <h2>Nueva Tarea</h2>
    <p v-if="error" class="error">{{ error }}</p>
    <input v-model="title" placeholder="Título *" />
    <textarea v-model="description" placeholder="Descripción (opcional)"></textarea>
    <button @click="submit">Agregar Tarea</button>
  </div>
</template>

<style scoped>
h2 {
 
  color: green;
  font-size: 2rem;
  font-family:Georgia, serif; ;
}
.form{
     display: flex;
    flex-direction: column; /* Alinea los elementos verticalmente */
    gap: 10px; /* Espacio entre elementos */
    width: 300px; 
}
</style>