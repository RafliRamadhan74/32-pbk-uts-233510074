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
  <div class="w-screen h-screen bg-gray-900 text-white flex flex-col items-center justify-center p-6 overflow-hidden">
    <div class="w-full h-100 max-w-xl bg-gray-800 rounded-2xl shadow-lg p-6 flex flex-col gap-4 animate-fade-in">
      
      <h1 class="text-3xl font-bold text-center mb-2">📝 To-Do List</h1>

      <div class="flex gap-2">
        <input
          type="text"
          v-model="newTask"
          placeholder="Add a new task"
          @keyup.enter="addTask"
          class="flex-1 p-2 rounded-lg bg-gray-700 text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-purple-500"
        />
        <button
          @click="addTask"
          class="bg-purple-600 hover:bg-purple-700 transition-all px-4 py-2 rounded-lg font-semibold"
        >
          Add
        </button>
      </div>

      <div class="flex justify-end">
        <select
          v-model="filter"
          class="bg-gray-700 text-white p-2 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500"
        >
          <option value="all">All</option>
          <option value="completed">Completed</option>
          <option value="incompleted">Incomplete</option>
        </select>
      </div>

      <div class="flex-1 overflow-y-auto max-h-64 custom-scrollbar pr-2">
        <ul class="flex flex-col gap-2">
          <li
            v-for="task in filteredTask"
            :key="task.id"
            class="flex items-center justify-between bg-gray-700 p-3 rounded-lg transition-transform hover:scale-[1.01] hover:bg-gray-600"
          >
            <div class="flex items-center gap-2">
              <input
                type="checkbox"
                v-model="task.completed"
                @change="toggleTask(task)"
                :checked="task.completed"
                class="form-checkbox h-5 w-5 text-purple-500"
              />
              <span :class="{'line-through text-gray-400': task.completed}">
                {{ task.text }}
              </span>
            </div>
            <button
              @click="removeTask(task)"
              class="text-red-400 hover:text-red-600 transition"
            >
              ✕
            </button>
          </li>
        </ul>
      </div>
      
    </div>
  </div>
</template>

<style scoped>
@keyframes fade-in {
  0% { opacity: 0; transform: scale(0.95); }
  100% { opacity: 1; transform: scale(1); }
}

.animate-fade-in {
  animation: fade-in 0.5s ease-out;
}


.custom-scrollbar::-webkit-scrollbar {
  width: 6px;
}

.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  background-color: #7e22ce; 
  border-radius: 8px;
}
</style>
