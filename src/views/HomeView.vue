<script setup>
import { ref } from 'vue'

const todoInput = ref('')
const todos = ref([])

const addTodo = () => {
  if (!todoInput.value) {
    alert('Please enter a todo!')
    todoInput.value = ''
    return
  }
  todos.value.push({
    id: todos.value.length + 1,
    description: todoInput.value,
    timestamp: new Date().toLocaleString()
  })
  todoInput.value = ''
}

const removeTodo = (idx) => {
  todos.value.find((todo, index) => {
    if (todo.id === idx) {
      todos.value.splice(index, 1)
    }
  })
}
</script>

<template>
  <main class="flex flex-col justify-center w-[400px] text-lg">
    <div class="grid grid-cols-12 gap-x-3">
      <div class="col-span-8">
        <div class="flex flex-col">
          <label for="todoInput" clasS="text-white mb-2 font-bold">TODO</label>
          <input 
            id="todoInput"
            type="text"
            v-model="todoInput"
            placeholder="Enter your todo here"
            class="h-[40px] text-black px-2"
            @keyup.enter="addTodo">
        </div>
      </div>
      <div class="col-span-4 flex items-end">
        <button 
          class="h-[40px] w-full bg-white text-black rounded-sm"
          @click="addTodo">Add Todo</button>
      </div>
    </div>
    <!-- todo container -->
    <div class="mt-2 border border-white rounded-md text-white p-2 max-h-[500px] overflow-auto">
      <template
        v-if="todos.length"
      >
        <div class="p-2 border-b-2 border-white flex flex-col items-start" v-for="(todo, idx) in todos" :key="idx">
            <div class="mb-2 text-sm flex flex-row w-full">
              <div class="flex-grow">
                Todo #{{ todo.id }}
              </div>
              <div class="text-white font-bold text-xl cursor-pointer" @click="removeTodo(todo.id)">&times;</div>
            </div>
            <div class="break-all mb-3">
              {{ todo.description }}
            
            </div>
            <div class="text-xs self-end">{{ todo.timestamp }}</div>
        </div>
      </template>
      <div v-else class="text-center">Nothing to display.</div>
    </div>
  </main>
</template>
