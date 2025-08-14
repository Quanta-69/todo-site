<script setup>
import { ref, onMounted, watch } from 'vue'

const todos = ref([])
const newTodo = ref('')

watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos))
}, { deep: true })

onMounted(() => {
  const savedTodos = localStorage.getItem('todos')
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos)
  }
})

function addTodo() {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ text: newTodo.value, completed: false })
    newTodo.value = ''
  }
}

function removeTodo(index) {
  todos.value.splice(index, 1)
}
</script>

<template>
  <div class="bg-gray-900 min-h-screen flex flex-col items-center text-white">
    <h1 class="text-4xl font-bold mt-20 mb-10 text-yellow-400">Todo App</h1>
    <div class="w-full max-w-md">
      <form @submit.prevent="addTodo" class="flex items-center mb-4">
        <input v-model="newTodo"
          class="flex-grow p-3 bg-gray-800 border-2 border-gray-700 rounded-l-lg focus:outline-none focus:border-yellow-400"
          placeholder="Add a new todo">
        <button
          class="p-3 bg-yellow-400 text-gray-900 font-bold rounded-r-lg hover:bg-yellow-500">Add</button>
      </form>
      <ul>
        <li v-for="(todo, index) in todos" :key="index"
          class="flex items-center justify-between p-3 mb-2 bg-gray-800 rounded-lg">
          <div class="flex items-center">
            <input type="checkbox" v-model="todo.completed" class="mr-3 h-5 w-5">
            <span :class="{ 'line-through text-gray-500': todo.completed }">{{ todo.text }}</span>
          </div>
          <button @click="removeTodo(index)"
            class="p-2 text-red-500 hover:text-red-700">Remove</button>
        </li>
      </ul>
    </div>
  </div>
</template>
