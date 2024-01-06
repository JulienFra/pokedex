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
      typePokemon {
        nom
      }
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

const searchQuery = ref("");
const pokemons = ref();
const selectedPokemon = ref(null);
const selectedType = ref("");
const { data } = await useAsyncQuery(query);
console.log(data.value);
pokemons.value = data.value.pokemons;

const showDetails = (pokemon) => {
  selectedPokemon.value = pokemon;
};

const filteredPokemons = computed(() => {
  // Filtrer les pokémons en fonction de la recherche
  let filteredList = pokemons.value;
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase();
    filteredList = filteredList.filter((pokemon) =>
      pokemon.nom.toLowerCase().includes(query)
    );
  }

  // Filtrer les pokémons en fonction du type sélectionné
  if (selectedType.value) {
    filteredList = filteredList.filter(
      (pokemon) => pokemon.typePokemon.nom === selectedType.value
    );
  }

  return filteredList;
});

// Liste des types disponibles
const types = computed(() => {
  const typeSet = new Set();
  pokemons.value.forEach((pokemon) => typeSet.add(pokemon.typePokemon.nom));
  return Array.from(typeSet);
});
</script>

<template>
  <div class="pokedex flex h-screen mb-12">
    <!-- Écran de gauche avec la liste des noms et la barre de recherche -->
    <div
      class="w-1/4 bg-gradient-to-r from-red-500 to-yellow-500 p-4 h-full overflow-auto"
      style="max-height: 70vh"
    >
      <!-- Barre de recherche -->
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Rechercher un Pokémon..."
        class="w-full p-2 mb-4 border-b-2 border-gray-300 focus:outline-none text-black sticky top-0"
      />

      <!-- Filtre par type -->
      <div class="mb-4 sticky top-10">
        <label for="typeFilter" class="text-black"
          >Filtrer par type principal:</label
        >
        <select
          v-model="selectedType"
          id="typeFilter"
          class="w-full p-2 border-b-2 border-gray-300 focus:outline-none text-black"
        >
          <option value="" selected>Tous les types</option>
          <option v-for="type in types" :key="type" :value="type">
            {{ type }}
          </option>
        </select>
      </div>

      <!-- Liste des noms de Pokémon -->
      <ul class="pokemon-list mb-4">
        <li v-for="pokemon in filteredPokemons" :key="pokemon.id" class="mb-4">
          <NuxtLink
            :to="`/pokemon/${pokemon.slug}`"
            class="block p-4 hover:bg-gray-200 rounded transition duration-300 ease-in-out shadow-md"
            @mouseover="showDetails(pokemon)"
          >
            {{ pokemon.nom }}
          </NuxtLink>
        </li>
      </ul>
    </div>

    <!-- Écran de droite avec l'image et le nom du Pokémon sélectionné -->
    <div class="pokemon-details flex-1 ml-4 h-full" style="max-height: 70vh">
      <div
        v-if="selectedPokemon"
        class="flex justify-center items-center w-full h-full"
      >
        <NuxtImg
          :src="selectedPokemon.image.url"
          :alt="selectedPokemon.nom"
          class="object-contain h-3/6 border-8 border-green-400"
        />
      </div>
      <div v-else class="text-center mt-4">
        <p class="text-gray-600">
          Survolez un Pokémon pour afficher les détails.
        </p>
      </div>
    </div>
  </div>
</template>
