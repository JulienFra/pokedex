<template>
  <div
    class="min-h-screen bg-cover bg-center bg-no-repeat relative"
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
  </div>
</template>
