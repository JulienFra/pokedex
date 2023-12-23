<script setup>
const query = gql`
  query Pokemons {
    pokemons(orderBy: id_ASC) {
      createdAt
      description
      id
      nom
      publishedAt
      slug
      updatedAt
      image {
        url(
          transformation: {
            document: { output: { format: webp } }
            image: { resize: { fit: crop, height: 256, width: 256 } }
          }
        )
      }
    }
  }
`;

const pokemons = ref();
const selectedPokemon = ref(null);
const { data } = await useAsyncQuery(query);
console.log(data.value);
pokemons.value = data.value.pokemons;

const showDetails = (pokemon) => {
  selectedPokemon.value = pokemon;
};
</script>

<template>
  <div class="pokedex flex">
    <!-- Écran de gauche avec la liste des noms -->
    <ul class="pokemon-list overflow-auto w-1/4 bg-red-300 p-4 h-96">
      <li v-for="pokemon in pokemons" :key="pokemon.id" class="mb-4">
        <NuxtLink
          :to="`/pokemon/${pokemon.slug}`"
          class="block p-2 hover:bg-gray-200 rounded transition duration-300 ease-in-out"
          @mouseover="showDetails(pokemon)"
        >
          {{ pokemon.nom }}
        </NuxtLink>
      </li>
    </ul>

    <!-- Écran de droite avec l'image et le nom du Pokémon sélectionné -->
    <div class="pokemon-details flex-1 ml-4">
      <div
        v-if="selectedPokemon"
        class="flex justify-center text-center w-full h-96"
      >
        <NuxtImg
          :src="selectedPokemon.image.url"
          :alt="selectedPokemon.nom"
          class="object-contain h-full"
        />
      </div>
      <div v-else class="text-center">
        <p class="mt-4">Survolez un Pokémon pour afficher les détails.</p>
      </div>
    </div>
  </div>
</template>
