<script setup>
import { ref, onMounted } from 'vue';
import api from './utils/api';
import Search from "./components/Search.vue"

const todos = ref([]);
const newTodo = ref('');

onMounted(() => {
  get();
});

async function get() {
  await api.get('/todos').then((response) => {
    todos.value = response.data;
  });
}

async function insertTodo() {
  await api.post("/todos", {
    id: (todos.value.length + 1).toString(),
    title: newTodo.value,
    completed: false,
  }).then(() => {
    get();
    newTodo.value = '';
  });
}

async function updateTodo(todo) {
  await api.patch(`/todos/${todo.id}`, {
    completed: !todo.completed
  }).then(() => {
    get();
  });
}

async function deleteTodo(todo) {
  await api.delete(`/todos/${todo.id}`).then(() => {
    get();
  });
}

function searching(searched) {
  if (!searched.length) {
    get()
    return;
  }

  todos.value = todos.value.filter((item) =>
    item.title.toLowerCase().includes(searched.toLowerCase())
  );
}
</script>

<template>
  <div class="container">
    <div class="flex flex-col gap-3">
      <h1 class="text-center text-2xl">To Do</h1>

      <div class="flex">
        <input type="text" placeholder="Adicionar nova tarefa"
          class="border border-gray-300 rounded-lg text-lg p-3 w-full focus:outline-none focus:ring-2 focus:ring-green-400 transition-all duration-200"
          v-model="newTodo">
        <button @click="insertTodo"
          class="ml-2 bg-green-500 hover:bg-green-600 text-white p-3 rounded-lg shadow transition-all duration-200">
          <img src="./assets/images/add.svg" alt="add_img" width="30" />
        </button>
      </div>
      <Search @searching-toDo="searching" />
      <div v-for="todo in todos" :key="todo.id"
        class="flex items-center justify-between border border-gray-300 rounded text-1xl p-2">
        <div class="flex items-center gap-3">
          <button v-if="todo.completed" @click="updateTodo(todo)">
            <img src="./assets/images/completed.svg" alt="Ok" width="15" />
          </button>
          <button v-else @click="updateTodo(todo)">
            <img src="./assets/images/notCompleted.svg" alt="not ok" width="15" />
          </button>
          <span>{{ todo.title }}</span>
        </div>
        <button @click="deleteTodo(todo)" class="text-red-500">
          Deletar
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  margin-left: auto;
  margin-right: auto;
  padding: 1.25rem 2.5rem;
}
</style>
