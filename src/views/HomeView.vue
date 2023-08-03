<script setup>
import { ref, nextTick } from 'vue'

const todoInput = ref('')
const todos = ref([])
const editInputRefs = ref([])

const isInputEmpty = (inputField) => {
  if (!inputField) {
    alert('Please enter a todo!')
    return true
  }
  return false
}

const addTodo = () => {
  if (isInputEmpty(todoInput.value)) return

  todos.value.push({
    isEdit: false,
    id: todos.value.length ? todos.value[todos.value.length - 1].id + 1 : 1,
    description: todoInput.value,
    timestamp: new Date().toLocaleString()
  })
  todoInput.value = ''
}

const removeTodo = (idx) => {
  todos.value.find((todo, index) => {
    if (todo && (todo.id === idx)) {
      todos.value.splice(index, 1)
    }
  })
}

const showEditTodo = (idx) => {
  if (isInputEmpty(todos.value[idx].description)) return

  todos.value[idx].isEdit = !todos.value[idx].isEdit
  nextTick(() => {
    if (todos.value[idx].isEdit) {
      editInputRefs.value[idx].focus()
    }
  })
}
</script>

<template>
  <main class="flex flex-col justify-center w-[500px] text-lg max-w-[90vw]">
    <img src="@/assets/logo.jpg" alt="logo" class="mb-10 border border-white">
    <div class="grid grid-cols-12 gap-x-3">
      <div class="col-span-8">
        <div class="flex flex-col">
          <label for="todoInput" clasS="text-white mb-3 font-bold text-3xl">TODO</label>
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
    <div class="mt-2 border border-white rounded-md text-white p-2 max-h-[350px] overflow-auto">
      <template
        v-if="todos.length"
      >
        <div class="p-2 border-b-2 border-white flex flex-col items-start" v-for="(todo, idx) in todos" :key="idx">
            <div class="mb-2 text-sm flex flex-row w-full">
              <div class="flex-grow font-extrabold">
                TODO #{{ todo.id }}
              </div>
              <div class="text-white font-bold text-xl cursor-pointer" @click="removeTodo(todo.id)">&times;</div>
            </div>
            <div v-show="!todo.isEdit" class="break-all mb-3 w-full cursor-pointer" @click="showEditTodo(idx)">
              {{ todo.description }}
            
            </div>
            <div v-show="todo.isEdit" class="break-all mb-3 w-full flex flex-col">
              <input 
                type="text"
                v-model="todo.description"
                placeholder="Enter your todo here"
                class="h-[40px] text-black px-2 w-full"
                ref="editInputRefs"
                @keyup.enter="showEditTodo(idx)">
                <div class="italic text-xs">Press enter to update</div>
            </div>
            <div class="text-xs font-bold tracking-widest self-end">{{ todo.timestamp }}</div>
        </div>
      </template>
      <div v-else class="text-center">Nothing to display.</div>
    </div>
  </main>
</template>
