<script setup>
import PokemonBall from "../../src/assets/pokemon-ball.png";
defineProps({
  pokemonData: {
    type: Object,
    required: true,
  },
});
</script>
<template>
  <div v-if="pokemonData" class="card w-full border mt-10">
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
  <div v-else class="mt-4 flex justify-center">
    <div>
      <img :src="PokemonBall" alt="Pokemon Ball" class="w-36" />
      <p class="mt-3 text-red-500 font-bold">Pokemon not found</p>
    </div>
  </div>
</template>
