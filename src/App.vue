<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filter = ref('all')

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

const removeTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

const filteredTask = computed(() =>{
  if(filter.value === "completed"){
    return tasks.value.filter(t => t.completed)
  }else if (filter.value === "incompleted"){
    return tasks.value.filter(t => !t.completed)
  }else {
    return tasks.value
  }
})

</script>

<template>
  <input type="text" name="" v-model="newTask" placeholder="Add a new task" @keyup.enter="addTask">
  <button @click="addTask">add task</button>

  <select v-model="filter">
    <option value="all">All</option>
    <option value="completed">Completed</option>
    <option value="incompleted">incompleted</option>
  </select>

  <ul>
    <li v-for="task in filteredTask" :key="task.id">
      <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" :checked="task.completed">
      {{ task.text }}
      <button @click="removeTask(task)">Remove</button>
    </li>
  </ul>
</template>

<style scoped></style>
