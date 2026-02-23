<script setup>
import { ref, onMounted, computed } from 'vue'
import TaskForm from '../components/TaskForm.vue'
import TaskItem from '../components/TaskItem.vue'

const tasks = ref([])
const filter = ref('all')
const message = ref('')

const filteredTasks = computed(() => {
  if (filter.value === 'completed') return tasks.value.filter(t => t.completed)
  if (filter.value === 'pending') return tasks.value.filter(t => !t.completed)
  return tasks.value
})

async function loadTasks() {
  const res = await fetch('http://localhost:8000/api/tasks')
  tasks.value = await res.json()
}

function onTaskCreated(task) {
  tasks.value.unshift(task)
  showMessage('Tarea creada ✓')
}

async function toggleTask(task) {
  const res = await fetch(`http://localhost:8000/api/tasks/${task.id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ ...task, completed: !task.completed })
  })
  const updated = await res.json()
  const index = tasks.value.findIndex(t => t.id === task.id)
  tasks.value[index] = updated
}

async function deleteTask(id) {
  await fetch(`http://localhost:8000/api/tasks/${id}`, { method: 'DELETE' })
  tasks.value = tasks.value.filter(t => t.id !== id)
  showMessage('Tarea eliminada')
}

function showMessage(msg) {
  message.value = msg
  setTimeout(() => message.value = '', 3000)
}

onMounted(loadTasks)
</script>

<template>
  <main>
    <h1 style="color:red;" >Gestor de Tareas</h1 >
    <p v-if="message" class="success">{{ message }}</p>
    <TaskForm @task-created="onTaskCreated" />
    <div class="filters">
      <button @click="filter = 'all'">Todas</button>
      <button @click="filter = 'pending'">Pendientes</button>
      <button @click="filter = 'completed'">Completadas</button>
    </div>
    <TaskItem
      v-for="task in filteredTasks"
      :key="task.id"
      :task="task"
      @toggle="toggleTask"
      @delete="deleteTask"
    />
    <p v-if="filteredTasks.length === 0">No hay tareas.</p>
  </main>
</template>

