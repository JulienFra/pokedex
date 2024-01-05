<script setup>
const query = gql`
  query Localisation($slug: String!) {
    localisation(where: { slug: $slug }) {
      nom
      image {
        url
      }
      pokemons {
        nom
        slug
        image {
          url
        }
      }
    }
  }
`;

const localisation = ref();

const route = useRoute();
const { data } = await useAsyncQuery(query, {
  slug: route.params.slug,
});

console.log(data.value);
localisation.value = data.value.localisation;
</script>

<template>
  <div v-if="localisation" class="flex p-8 bg-white shadow-lg rounded-lg mb-8">
    <!-- Left Section: Localisation Image -->
    <div class="w-1/2 pr-8">
      <NuxtImg
        :src="localisation.image.url"
        :alt="localisation.nom"
        class="w-full h-auto rounded-lg"
      />
    </div>

    <!-- Right Section: Localisation Name and List of Pokémons -->
    <div class="w-1/2 bg-gray-100 p-8 rounded">
      <!-- Localisation Name at the Top -->
      <h2 class="text-3xl mb-4 text-gray-800">{{ localisation.nom }}</h2>

      <!-- List of Pokémons -->
      <ul>
        <li
          v-for="pokemon in localisation.pokemons"
          :key="pokemon.nom"
          class="mb-4"
        >
          <!-- Updated styling for the Pokémon "button" using NuxtLink -->
          <NuxtLink
            :to="`/pokemon/${pokemon.slug}`"
            class="flex items-center p-3 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition duration-300"
          >
            <NuxtImg
              :src="pokemon.image.url"
              :alt="pokemon.nom"
              class="w-8 h-8 mr-2 rounded-full"
            />
            <span class="text-lg">{{ pokemon.nom }}</span>
          </NuxtLink>
        </li>
      </ul>
    </div>
  </div>
  <div v-else class="text-center mt-8">
    <p class="text-xl text-gray-600">Loading...</p>
  </div>
</template>
