<script setup>
import { onMounted, ref, watch } from "vue";

let todosData = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos/");
    const data = await response.json();

    todosData.value = data;
  } catch (error) {
    console.error("Error fetching data:", error); // Handle any errors
  }
});
const deleteTodo = (id) => {
  todosData.value = todosData.value.filter((todo) => todo.id !== id);
};
</script>

<template>
  <main class="p-5">
    <h1 class="text-2xl font-bold mt-5">PokemonAPI</h1>
    <div v-if="todosData.length">
      <ul class="flex justify-start flex-wrap">
        <li
          class="p-5 bg-gray-300 m-2 rounded-lg"
          v-for="todo in todosData"
          :key="todo.id"
        >
          {{ todo.title }}
          <button class="bg-red-500 p-2 rounded-lg" @click="deleteTodo(todo.id)">
            Delete
          </button>
        </li>
      </ul>
    </div>
  </main>
</template>
