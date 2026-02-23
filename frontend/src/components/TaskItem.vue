<script setup>
defineProps(['task'])
const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <div class="task-item" :class="{ completed: task.completed }">
    <input type="checkbox" :checked="task.completed" @change="emit('toggle', task)" />
    <div>
      <strong>{{ task.title }}</strong>
      <p v-if="task.description">{{ task.description }}</p>
    </div>
    <button @click="emit('delete', task.id)">Eliminar</button>
  </div>
</template>

<style scoped> 
strong{
    color:black;
}
p{
    color:black
}
</style>