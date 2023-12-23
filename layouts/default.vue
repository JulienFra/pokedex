<template>
  <div
    class="min-h-screen bg-cover bg-center bg-no-repeat relative"
    style="
      background-image: url('/images/_9c2088ca-b682-43dc-9370-439a88a262a9.jpg');
    "
  >
    <nav class="container mx-auto px-3 h-24 flex items-center mb-10">
      <NuxtLink to="/">
        <h1 class="text-5xl text-white">Pokedex</h1>
      </NuxtLink>
    </nav>
    <div class="container mx-auto px-3 text-white">
      <slot />
    </div>
  </div>
</template>
