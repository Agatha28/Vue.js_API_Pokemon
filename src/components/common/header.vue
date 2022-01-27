<template>
  <!-- This example requires Tailwind CSS v2.0+ -->
  <div class="relative bg-white">
    <div class="px-4 mx-auto max-w-7xl sm:px-6">
      <div class="flex items-center justify-between py-6 border-b-2 border-gray-100 md:justify-start md:space-x-10">
        <div class="flex justify-start lg:w-0 lg:flex-1">
          <a href="#">
            <span class="sr-only">Workflow</span>
            <img class="w-auto h-8 sm:h-10" src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/320px-International_Pok%C3%A9mon_logo.svg.png" alt="">
          </a>
        </div>
        <nav class="hidden space-x-10 md:flex">
          <router-link :to="{ name: 'Home'}" class="text-base font-medium text-gray-500 hover:text-blue-900">
            Home
          </router-link>
          <router-link :to="{ name: 'PokemonList'}" class="text-base font-medium text-gray-500 hover:text-blue-900">
            Detail Pokemon
          </router-link>
        </nav>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeaderComponent',
  props: {
    msg: String,
  },
};
</script>
