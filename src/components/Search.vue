<script setup>
import { ref } from 'vue';

const searched = ref('');

const emit = defineEmits(["searching-toDo"]);

function searching() {
  emit("searching-toDo", searched.value);
}
</script>

<template>
  <input type="text" placeholder="Procurar To Do" v-model="searched" 
    class="border border-gray-300 rounded text-1xl p-2" 
    @input="searching" />
</template>

<style scoped></style>
