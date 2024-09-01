<script setup>
import { onMounted, ref, watch } from "vue";
import Logo from "../src/assets/pokemon-logo.png";
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

console.log(pokemonData.value);
</script>

<template>
  <main class="flex justify-center items-center p-5">
    <div class="lg:w-1/2">
      <div class="flex justify-center">
      <img :src="Logo" alt="Pokemon Logo" class="mb-10">
       
      </div>
      <label>Search Pokemon</label>
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
        <div v-if="pokemonData" class="card w-96 border mt-10">
          <figure class="bg-secondary">
            <img
              :src="pokemonData.sprites.front_default"
              :alt="pokemonData.name"
              class="w-52"
            />
          </figure>
          <div className="card-body">
            <p class="mt-3"><strong>Name:</strong> {{ pokemonData.name }}</p>
            <p><strong>Base Experience:</strong> {{ pokemonData.base_experience }}</p>
            <p><strong>Height:</strong> {{ pokemonData.height }}</p>
            <p><strong>Weight:</strong> {{ pokemonData.weight }}</p>
          </div>
        </div>
        <div v-else class="text-center mt-4">
          <p class="mt-3 text-red-500">Pokemon not found</p>
        </div>
      </div>
    </div>
  </main>
</template>
