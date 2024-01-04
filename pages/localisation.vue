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
  <ul
    v-if="localisations"
    class="grid gap-8 grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 2xl:grid-cols-6"
  >
    <li v-for="localisation in localisations" :key="localisation.slug">
      <NuxtLink :to="`/localisation/${localisation.slug}`">
        <NuxtImg :src="localisation.image.url" :alt="localisation.nom" />
        <h2 class="text-3xl text-center">{{ localisation.nom }}</h2>
      </NuxtLink>
    </li>
  </ul>
  <ul v-else>
    <li>Loading...</li>
  </ul>
</template>
