<script setup>
const query = gql`
  query Localisations {
    localisations(orderBy: nom_ASC) {
      nom
      slug
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

const localisations = ref();
const { data } = await useAsyncQuery(query);
localisations.value = data.value.localisations;
</script>

<template>
  <div class="bg-gray-100 p-8 min-h-screen">
    <div class="max-h-screen overflow-y-auto mb-8">
      <!-- Ajout de max-h-screen et overflow-y-auto -->
      <ul
        v-if="localisations"
        class="grid gap-8 grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 2xl:grid-cols-8"
      >
        <li v-for="localisation in localisations" :key="localisation.slug">
          <NuxtLink :to="`/localisations/${localisation.slug}`">
            <div class="text-center">
              <NuxtImg
                :src="localisation.image.url"
                :alt="localisation.nom"
                class="w-full h-48 object-cover rounded-md mb-2"
              />
              <h2 class="text-lg text-black">{{ localisation.nom }}</h2>
            </div>
          </NuxtLink>
        </li>
      </ul>
      <ul v-else>
        <li class="text-center text-gray-600">Chargement en cours...</li>
      </ul>
    </div>
  </div>
</template>
