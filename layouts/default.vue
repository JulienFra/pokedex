<template>
  <div
    class="bg-cover bg-center bg-no-repeat relative"
    style="
      background-image: url('/images/_9c2088ca-b682-43dc-9370-439a88a262a9.jpg');
    "
  >
    <nav class="bg-gradient-to-r from-red-500 to-yellow-500">
      <div class="container mx-auto px-4 h-24 flex items-center mb-10">
        <NuxtLink to="/">
          <h1 class="text-4xl text-white font-bold mr-8">Pokedex</h1>
        </NuxtLink>
        <NuxtLink to="/localisation">
          <h2 class="text-2xl text-white">Voir les localisations</h2>
        </NuxtLink>
      </div>
    </nav>
    <div class="container mx-auto px-3 text-white">
      <slot />
    </div>
    <footer class="bg-gray-800 text-white text-center py-4">
      <button
        @click="showContactPopup"
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
      >
        Contact
      </button>
      <!-- Contact Popup -->
      <div
        v-if="showPopup"
        class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-75"
      >
        <div class="bg-white p-8 max-w-md mx-auto rounded-md">
          <h2 class="text-2xl font-bold mb-4 text-black">Contactez-nous</h2>
          <p class="mb-4 text-black">
            Retrouvez-nous sur les réseaux sociaux :
          </p>
          <div class="flex justify-around">
            <a href="#" class="text-blue-500 hover:underline">Facebook</a>
            <a href="#" class="text-blue-400 hover:underline">Twitter</a>
            <a href="#" class="text-red-500 hover:underline">Instagram</a>
          </div>
          <button
            @click="closeContactPopup"
            class="mt-8 bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded"
          >
            Fermer
          </button>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
const showPopup = ref(false);

const showContactPopup = () => {
  showPopup.value = true;
};

const closeContactPopup = () => {
  showPopup.value = false;
};
</script>
