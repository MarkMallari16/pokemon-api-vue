<script setup>
import { onMounted, ref, watch } from "vue";
import Logo from "../src/assets/pokemon-logo.png";

import PokemonCard from "./components/PokemonCard.vue";
import ChildComponent from "./components/ChildComponent.vue";

const pokemonData = ref([]);
const searchQuery = ref("pikachu");
const loading = ref(true);

const fetchedPokemonData = async (query) => {
  loading.value = true;
  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${query.toLowerCase()}`
    );
    if (response.ok) {
      const data = await response.json();

      pokemonData.value = data;
      console.log(data);
    } else {
      pokemonData.value = null;
    }
  } catch (error) {
    console.error("Error fetching data:", error); // Handle any errors
  } finally {
    loading.value = false;
  }
};
onMounted(() => {
  fetchedPokemonData(searchQuery.value);
});

watch(searchQuery, (newQuery) => {
  if (newQuery) {
    fetchedPokemonData(newQuery);
  }
});
</script>

<template>
  <main class="flex justify-center items-center p-5 min-h-screen bg-blue-50">
    <div class="lg:w-1/2">
      <div class="flex justify-center">
        <img :src="Logo" alt="Pokemon Logo" class="mb-10" />
      </div>
      <label>Search Pokemon Species</label>
      <div class="relative">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          fill="currentColor"
          class="size-6 absolute inset-5 text-slate-700"
        >
          <path
            fill-rule="evenodd"
            d="M10.5 3.75a6.75 6.75 0 1 0 0 13.5 6.75 6.75 0 0 0 0-13.5ZM2.25 10.5a8.25 8.25 0 1 1 14.59 5.28l4.69 4.69a.75.75 0 1 1-1.06 1.06l-4.69-4.69A8.25 8.25 0 0 1 2.25 10.5Z"
            clip-rule="evenodd"
          />
        </svg>

        <input
          type="text"
          v-model="searchQuery"
          class="mt-2 pl-14 input input-bordered w-full"
          placeholder="Search pokemon..."
        />
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          fill="currentColor"
          class="size-6 absolute top-5 right-5 cursor-pointer"
          v-if="searchQuery"
          @click="searchQuery = ''"
        >
          <path
            fill-rule="evenodd"
            d="M5.47 5.47a.75.75 0 0 1 1.06 0L12 10.94l5.47-5.47a.75.75 0 1 1 1.06 1.06L13.06 12l5.47 5.47a.75.75 0 1 1-1.06 1.06L12 13.06l-5.47 5.47a.75.75 0 0 1-1.06-1.06L10.94 12 5.47 6.53a.75.75 0 0 1 0-1.06Z"
            clip-rule="evenodd"
          />
        </svg>
      </div>
      <div v-if="loading" class="mt-4">
        <p class="text-center">Loading....</p>
      </div>
      <div v-else>
        <PokemonCard :pokemonData="pokemonData" />
      </div>
    </div>
  </main>
  <ChildComponent>
    <template v-slot:footer>
      <h1 class="text-center text-gray-600">&copy; Mark Mallari</h1>
    </template>
  </ChildComponent>
</template>
