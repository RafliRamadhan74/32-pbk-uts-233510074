<script setup>
import { ref } from 'vue'

const tasks = ref([])
const newTask = ref('')

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const toggleTask = (task) => {
  task.completed == !task.completed
  console.log(task.completed)
  console.log(tasks.value)
}

</script>

<template>
  <input type="text" name="" v-model="newTask" placeholder="Add a new task" @keyup.enter="addTask">
  <button @click="addTask">add task</button>

  <ul>
    <li v-for="task in tasks" :key="task.id">
      <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" :checked="task.completed"> 
      {{ task.text }}
    </li>
  </ul>
</template>

<style scoped></style>
