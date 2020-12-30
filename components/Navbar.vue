<template>
  <nav class="flex items-center flex-wrap bg-brown-200 px-2 lg:px-6 pt-0 shadow-lg w-full sticky top-0 justify-between lg:justify-center border-b border-brown-400 z-50">
    <nuxt-link title="Lior Cosmetics" to="/pedidos/">
      <img class="w-20 lg:w-24" src="../static/Logo.png">
      <!--<span class="block w-full text-brown-700 text-4xl text-center h-10">LIOR</span>
      <span class="block w-full text-brown-700 text-xs text-center">COSMETICS</span>-->
    </nuxt-link>
    <div class="flex lg:hidden">
      <button
        @click="toggle"
        class="flex items-center px-3 py-2 border rounded text-brown-800 border-brown-800 hover:text-brown-200 hover:border-brown-200 hover:bg-brown-800 focus:outline-none hove:outline-none"
      >
        <svg class="fill-current h-3 w-3" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
          <title>Menu</title>
          <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
        </svg>
      </button>
    </div>
    <div :class="open ? 'block': 'hidden'"
          class="w-full block flex-grow lg:flex lg:items-center lg:w-auto">
      <div class="text-sm lg:flex-grow">
        <nuxt-link title="Nuevo Cliente" to="/pedidos/" class="text-brown-800 float-right font-semibold pb-2 lg:pb-0 text-md lg:text-lg uppercase">
        Nuevo Cliente</nuxt-link>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "NavBar",
  data() {
    return {
      open: false,
    }
  },
  methods: {
    toggle() {
      this.open = !this.open;
    }
  },
  watch: {
    '$route' () {
      this.open = false
    }
  }
};
</script>

<style>

</style>
