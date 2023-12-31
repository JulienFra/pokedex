<script setup>
const query = gql`
  query Pokemon($slug: String!) {
    pokemon(where: { slug: $slug }) {
      id
      nom
      slug
      description
      createdAt
      publishedAt
      updatedAt
      stage
      image {
        url(
          transformation: {
            image: { resize: { fit: crop, height: 1024, width: 1024 } }
            document: { output: { format: webp } }
          }
        )
      }
      attaque1 {
        nom
        description
        degat
        type {
          logo {
            url
          }
        }
      }
      attaque2 {
        degat
        nom
        description
        type {
          logo {
            url
          }
        }
      }
      poid
      taille
      pv
      typePokemon {
        nom
        logo {
          url
        }
        faiblesse {
          nom
          logo {
            url
          }
        }
      }
      typePokemon2 {
        nom
        logo {
          url
        }
        faiblesse {
          nom
          logo {
            url
          }
        }
      }
      localisation {
        nom
        slug
        image {
          url
        }
      }
    }
  }
`;

const pokemon = ref();

const route = useRoute();
const { data } = await useAsyncQuery(query, {
  slug: route.params.slug,
});

console.log(data.value);
pokemon.value = data.value.pokemon;

const getTypeBackgroundColor = (type) => {
  switch (type) {
    case "Feu":
      return "#FFB3B3"; // Rouge pastel
    case "Eau":
      return "#B3E0FF"; // Bleu pastel
    case "Plante":
      return "#B3FFB3"; // Vert pastel
    case "Vol":
      return "#E0E0E0"; // Blanc pastel
    case "Electrique":
      return "#FFF9B3"; // Jaune pastel
    case "Poison":
      return "#D8B3FF";
    case "Roche":
      return "#D2B48C"; // Brun pastel
    default:
      return "#CCCCCC"; // Gris pastel par défaut si le type n'est pas géré
  }
};

// Utilisez la fonction getTypeBackgroundColor pour définir la couleur dynamiquement
const backgroundColor = getTypeBackgroundColor(pokemon.value.typePokemon.nom);

const isCardPopupVisible = ref(false);

const showCardPopup = () => {
  isCardPopupVisible.value = true;
};

const hideCardPopup = () => {
  isCardPopupVisible.value = false;
};
</script>

<template>
  <Head v-if="pokemon">
    <Title>{{ pokemon.nom }} - Détails du Pokémon</Title>
    <Meta
      name="description"
      :content="`Découvrez des détails sur ${pokemon.nom}: ${pokemon.description}`"
    />
    <Meta
      property="og:title"
      :content="`${pokemon.nom} - Détails du Pokémon`"
    />
    <Meta
      property="og:description"
      :content="`Découvrez des détails sur ${pokemon.nom}: ${pokemon.description}`"
    />
    <Meta property="og:image" :content="pokemon.image.url" />
    <Meta property="og:type" content="website" />
    <Meta property="og:locale" content="fr_FR" />
    <Meta name="twitter:card" content="summary_large_image" />
    <Meta
      name="twitter:title"
      :content="`${pokemon.nom} - Détails du Pokémon`"
    />
    <Meta
      name="twitter:description"
      :content="`Découvrez des détails sur ${pokemon.nom}: ${pokemon.description}`"
    />
    <Meta name="twitter:image" :content="pokemon.image.url" />
  </Head>
  <div>
    <div v-if="pokemon" class="flex">
      <!-- Pokémon Card -->
      <div
        class="max-w-xl bg-white rounded-lg overflow-hidden shadow-md border-8 border-yellow-500 relative mr-4 mb-4 transition-transform transform hover:scale-105"
        :style="`background-color:${backgroundColor}`"
        @click="showCardPopup"
      >
        <!-- Bannière avec le nom, les types, et les PV -->
        <div
          class="absolute top-0 left-0 right-0 bg-blue-500 p-2 text-white flex justify-between items-center"
        >
          <span class="text-xl font-semibold">{{ pokemon.nom }}</span>
          <div class="flex space-x-2">
            <NuxtImg
              :src="pokemon.typePokemon.logo.url"
              :alt="pokemon.typePokemon.nom"
              class="w-8 h-8 rounded-full"
            />
            <NuxtImg
              :src="pokemon.typePokemon2.logo.url"
              :alt="pokemon.typePokemon2.nom"
              class="w-8 h-8 rounded-full"
            />
            <span class="text-lg font-semibold ml-2">PV: {{ pokemon.pv }}</span>
          </div>
        </div>

        <!-- Image du Pokémon -->
        <div class="border-t border-b border-blue-500">
          <NuxtImg
            :src="pokemon.image.url"
            :alt="pokemon.nom"
            class="w-full h-full object-cover"
          />
        </div>

        <!-- Contenu de la carte -->
        <div class="p-4">
          <div class="mb-4">
            <h3 class="text-lg font-semibold mb-2 text-black">Attaques:</h3>
            <div class="flex flex-col space-y-2">
              <div class="flex items-center space-x-2">
                <NuxtImg
                  :src="pokemon.attaque1.type.logo.url"
                  :alt="pokemon.attaque1.nom"
                  class="w-8 h-8 rounded-full"
                />
                <div>
                  <h4 class="text-md font-medium text-black">
                    {{ pokemon.attaque1.nom }} -
                    {{ pokemon.attaque1.degat }} Dégâts
                  </h4>
                  <p class="text-sm text-black">
                    {{ pokemon.attaque1.description }}
                  </p>
                </div>
              </div>
              <div class="flex items-center space-x-2">
                <NuxtImg
                  :src="pokemon.attaque2.type.logo.url"
                  :alt="pokemon.attaque2.nom"
                  class="w-8 h-8 rounded-full"
                />
                <div>
                  <h4 class="text-md font-medium text-black">
                    {{ pokemon.attaque2.nom }} -
                    {{ pokemon.attaque2.degat }} Dégâts
                  </h4>
                  <p class="text-sm text-black">
                    {{ pokemon.attaque2.description }}
                  </p>
                </div>
              </div>
            </div>
          </div>

          <div class="mb-4">
            <h3 class="text-lg font-semibold mb-2 text-black">Faiblesses:</h3>
            <div class="flex items-center space-x-2">
              <NuxtImg
                :src="pokemon.typePokemon.faiblesse.logo.url"
                :alt="pokemon.typePokemon.faiblesse.nom"
                class="w-6 h-6 rounded-full"
              />
              <NuxtImg
                :src="pokemon.typePokemon2.faiblesse.logo.url"
                :alt="pokemon.typePokemon2.faiblesse.nom"
                class="w-6 h-6 rounded-full"
              />
            </div>
          </div>
        </div>

        <!-- Informations sur la taille et le poids en bas à droite -->
        <div class="absolute bottom-2 right-2 text-sm text-gray-600">
          <span class="text-xs text-black"> Poids: {{ pokemon.poid }} Kg </span>
          <span class="text-xs ml-2 text-black">
            Taille: {{ pokemon.taille }} m
          </span>
        </div>
      </div>

      <div
        v-if="isCardPopupVisible"
        class="fixed top-0 left-0 right-0 bottom-0 flex items-center justify-center bg-black bg-opacity-75"
        @click.stop="hideCardPopup"
      >
        <div
          class="max-w-md bg-white rounded-lg overflow-hidden shadow-md border-8 border-yellow-500 relative"
          :style="`background-color:${backgroundColor}`"
        >
          <div
            class="absolute top-0 left-0 right-0 bg-blue-500 p-2 text-white flex justify-between items-center"
          >
            <span class="text-xl font-semibold">{{ pokemon.nom }}</span>
            <div class="flex space-x-2">
              <NuxtImg
                :src="pokemon.typePokemon.logo.url"
                :alt="pokemon.typePokemon.nom"
                class="w-8 h-8 rounded-full"
              />
              <NuxtImg
                :src="pokemon.typePokemon2.logo.url"
                :alt="pokemon.typePokemon2.nom"
                class="w-8 h-8 rounded-full"
              />
              <span class="text-lg font-semibold ml-2"
                >PV: {{ pokemon.pv }}</span
              >
            </div>
          </div>

          <div class="border-t border-b border-blue-500">
            <NuxtImg
              :src="pokemon.image.url"
              :alt="pokemon.nom"
              class="w-full h-52 object-cover"
            />
          </div>

          <div class="p-4">
            <div class="mb-4">
              <h3 class="text-lg font-semibold mb-2 text-black">Attaques:</h3>
              <div class="flex flex-col space-y-2">
                <div class="flex items-center space-x-2">
                  <NuxtImg
                    :src="pokemon.attaque1.type.logo.url"
                    :alt="pokemon.attaque1.nom"
                    class="w-8 h-8 rounded-full"
                  />
                  <div>
                    <h4 class="text-md font-medium text-black">
                      {{ pokemon.attaque1.nom }} -
                      {{ pokemon.attaque1.degat }} Dégâts
                    </h4>
                    <p class="text-sm text-black">
                      {{ pokemon.attaque1.description }}
                    </p>
                  </div>
                </div>
                <div class="flex items-center space-x-2">
                  <NuxtImg
                    :src="pokemon.attaque2.type.logo.url"
                    :alt="pokemon.attaque2.nom"
                    class="w-8 h-8 rounded-full"
                  />
                  <div>
                    <h4 class="text-md font-medium text-black">
                      {{ pokemon.attaque2.nom }} -
                      {{ pokemon.attaque2.degat }} Dégâts
                    </h4>
                    <p class="text-sm text-black">
                      {{ pokemon.attaque2.description }}
                    </p>
                  </div>
                </div>
              </div>
            </div>

            <div class="mb-4">
              <h3 class="text-lg font-semibold mb-2 text-black">Faiblesses:</h3>
              <div class="flex items-center space-x-2">
                <NuxtImg
                  :src="pokemon.typePokemon.faiblesse.logo.url"
                  :alt="pokemon.typePokemon.faiblesse.nom"
                  class="w-6 h-6 rounded-full"
                />
                <NuxtImg
                  :src="pokemon.typePokemon2.faiblesse.logo.url"
                  :alt="pokemon.typePokemon2.faiblesse.nom"
                  class="w-6 h-6 rounded-full"
                />
              </div>
            </div>
          </div>

          <div class="absolute bottom-2 right-2 text-sm text-gray-600">
            <span class="text-xs text-black">
              Poids: {{ pokemon.poid }} Kg
            </span>
            <span class="text-xs ml-2 text-black">
              Taille: {{ pokemon.taille }} m
            </span>
          </div>
        </div>
      </div>

      <!-- Pokémon Description -->
      <div class="max-w-md mb-4">
        <h2 class="text-2xl font-semibold mb-2 text-black">
          {{ pokemon.nom }}
        </h2>
        <p
          :style="{
            backgroundColor: 'rgba(255, 255, 255, 0.8)',
            padding: '20px',
            borderRadius: '8px',
          }"
          class="text-lg text-black"
        >
          {{ pokemon.description }}
        </p>
        <div class="text-center">
          <h2 class="text-3xl font-semibold mb-4 text-gray-800">
            {{ pokemon.localisation.nom }}
          </h2>
          <!-- Ajoute la balise NuxtLink pour rendre l'image clickable -->
          <NuxtLink :to="`/localisations/${pokemon.localisation.slug}`">
            <NuxtImg
              :src="pokemon.localisation.image.url"
              :alt="pokemon.localisation.nom"
              class="border-4 rounded-lg shadow-md mx-auto w-96 cursor-pointer"
            />
          </NuxtLink>
        </div>
      </div>
    </div>
    <div v-else>
      <li>Loading...</li>
    </div>
  </div>
</template>
