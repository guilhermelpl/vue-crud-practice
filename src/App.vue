<script setup>
import { ref, onMounted } from 'vue';
import api from './utils/api';

const todos = ref([]);

onMounted(() => {
  api.get('/todos').then((response) => {
    todos.value = response.data;
  });
});
</script>

<template>
  <div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.title }}
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
