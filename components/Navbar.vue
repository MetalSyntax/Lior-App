<template>
  <nav class="flex items-center flex-wrap bg-blue-legacy px-6 pt-0 pb-2 shadow-lg w-full z-10 sticky top-0 justify-center">
    <nuxt-link to="/">
      <span class="block w-full text-white text-3xl text-center h-8">LIOR</span>
      <span class="block w-full text-white text-sm text-center">PEDIDOS</span>
    </nuxt-link>
    <!--<img src="../assets/img/logo.png" class="logo h-16" alt="logo">-->
    <!--<div class="flex items-center flex-shrink-0 text-white mr-6">
      <span class="font-semibold text-xl tracking-tight">Lior Cosmetics</span>
    </div>
    <div class="block lg:hidden">
      <button
        class="flex items-center px-3 py-2 border rounded text-teal-200 border-teal-400 hover:text-white hover:border-white"
      >
        <svg class="fill-current h-3 w-3" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
          <title>Menu</title>
          <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
        </svg>
      </button>
    </div>
    <div class="w-full block flex-grow lg:flex lg:items-center lg:w-auto">
      <div class="text-sm lg:flex-grow">
        <a
          href="#responsive-header"
          class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4"
        >Docs</a>
        <a
          href="#responsive-header"
          class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4"
        >Examples</a>
        <a
          href="#responsive-header"
          class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white"
        >Blog</a>
      </div>
    </div>-->
  </nav>
</template>

<script>
export default {
  name: "NavBar",
};
</script>

<style>
.bg-blue-legacy {
  background: #244a8b;
}
</style>
