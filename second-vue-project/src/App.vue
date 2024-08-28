<script setup>
import { onMounted, ref, watch } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";

const dragonBalls = ref({});

onMounted(async () => {
  try {
    const response = await fetch("https://dragonball-api.com/api/characters");
    const data = await response.json();

    dragonBalls.value = [data];
  } catch (error) {
    console.error("Failed to fetch data:", error);
  }
});
watch(dragonBalls, async (newDragonBalls) => {
  console.log(newDragonBalls);
});

console.log(dragonBalls.value);
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>
