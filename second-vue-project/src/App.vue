<script setup>
import { onMounted, ref, watch } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";

let todosData = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();


    todosData.value = data;
  } catch (error) {
    console.error("Error fetching data:", error); // Handle any errors
  }
});
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="Dragon Ball" />
    </div>
  </header>

  <main>
    <div v-if="todosData.length">
      <ul>
        <li v-for="todo in todosData" :key="todo.id">
          {{ todo.title }}
        </li>
      </ul>
    </div>
    <p v-else>Loading...</p>
  </main>
</template>
