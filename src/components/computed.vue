<script setup>
import { ref, computed } from 'vue'

// give each todo a unique id
let id = 0

const newTodo = ref('')
const hideComplemented = ref(false)
const todos = ref([
  { id: id++, text: 'Lamborgini', done: true},
  { id: id++, text: 'BMW', done: true},
  { id: id++, text: 'GTR', done: false}
])

const filteredTodos = computed(() => {
  return hideComplemented.value
  ? todos.value.filter((t) => !t.done)
  : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value})
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
  
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideComplemented = !hideComplemented">
    {{ hideComplement ? 'Show All' : 'Hide Complemented' }}
  </button>
</template>
<style>
.done {
  text-decoration: line-through;
}
</style>