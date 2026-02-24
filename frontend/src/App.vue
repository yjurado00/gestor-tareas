<script setup>
import { ref, onMounted, computed } from 'vue'
import TaskForm from './components/TaskForm.vue'
import TaskItem from './components/TaskItem.vue'

const tasks = ref([])
const filter = ref('all') // 'all', 'pending', 'completed'
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
  <div class="container">
    <h1>Gestor de Tareas</h1>
    
    <img src="https://img.icons8.com/?size=100&id=OMinisxEXp1h&format=png&color=000000" alt="">
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
  </div>
</template>

<style scoped>

.container {
 max-width: 600px;       /* controla el ancho */
  margin: -200px;      /* CENTRA horizontal */
  padding: 20px;          /* espacio interno */
  background: white;
  border-radius: 10px;
}


.main {
text-align: center;
background:white;
}

h1 {
  text-align: center;
  color: green;
  font-size: 3rem;
  font-family:Georgia, serif; ;
}
img{
   display: block;
    margin-left: auto;
    margin-right: auto;
    width: 30%; 
}

p{
  color:black;
}

</style>