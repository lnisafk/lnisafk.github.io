<script setup>
import { ref, computed } from 'vue'

const titleClass = ref('title')
const named = ref(0)
const name = ref("")
function submit() {
  if (name.value === "Ni")
    named.value = 2
  else if (name.value === "Rukia")
    named.value = 3
  else
    named.value = 1
}
function rename() {
  named.value = 0
}

let id = 0;
const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: '背单词', done: false },
  { id: id++, text: '做数学题', done: false },
  { id: id++, text: '学习vue', done: false }
])
const filteredTodos = computed(() =>{
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})
function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false})
  newTodo.value = ''
}
function removeTodo(item) {
  todos.value = todos.value.filter((t) => t !== item)
}


</script>

<template>
  <h1 :class="titleClass">Nice to Meet You</h1>

  <input v-model="name" placeholder="who are you">
  <button @click="submit">submit</button>
  <button @click="rename">rename</button>

  <li v-if="named === 0">Please enter your name.</li>
  <li v-else-if="named === 1">Hello, {{ name }}!</li>
  <ul v-else-if="named === 2">
    <li>Welcome home.</li>
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" required placeholder="new todo">
      <button>Add Todo</button>
    </form>
    <ul>
      <li v-for="item in filteredTodos" :key="item.id">
        <input type="checkbox" v-model="item.done">
        <span :class="{ done: item.done }">{{ item.text }}</span>
        <button @click="removeTodo(item)">Done</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
  </ul>
  <ul v-else-if="named === 3">
    <a :href="downloadUrl" download="snl.zip">click to download</a>
  </ul>
</template>

<script>
export default {
  data() {
    return {
        downloadUrl: 'https://raw.githubusercontent.com/lnisafk/snl/main/snl.zip',
    };
  },
};
</script>

<style>
.title {
  color: green;
}
.done {
  text-decoration: line-through;
}
</style>