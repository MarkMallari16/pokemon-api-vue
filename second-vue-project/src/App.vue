<script setup>
import { onMounted, ref, watch } from "vue";
import Logo from "../src/assets/pokemon-logo.png";
import PokemonCard from "./components/PokemonCard.vue";

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
  <main class=" flex justify-center items-center p-5 min-h-screen">
    <div class="lg:w-1/2">
      <div class="flex justify-center">
      <img :src="Logo" alt="Pokemon Logo" class="mb-10">
       
      </div>
      <label>Search Pokemon Species</label>
      <input
        type="text"
        v-model="searchQuery"
        class="mt-2 input input-bordered w-full"
        placeholder="Search pokemon..."
      />
      <div v-if="loading" class="mt-2">
        <p class="text-center">Loading....</p>
      </div>
      <div v-else >
        <PokemonCard :pokemonData="pokemonData"/>
      </div>
    </div>
  </main>
</template>
