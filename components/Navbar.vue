<template>
  <nav class="flex items-center flex-wrap bg-blue-custom px-6 pt-0 pb-2 shadow-lg w-full z-10 sticky top-0 justify-between lg:justify-center">
    <nuxt-link to="/">
      <span class="block w-full text-white text-3xl text-center h-8">LIOR</span>
      <span class="block w-full text-white text-sm text-center">PEDIDOS</span>
    </nuxt-link>
    <div class="flex lg:hidden">
      <button
        @click="toggle"
        class="flex items-center px-3 py-2 border rounded text-white border-white hover:text-white hover:border-white focus:outline-none hove:outline-none"
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
        <nuxt-link to="/" class="text-white float-right">
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
.bg-blue-custom {
  background: #244a8b;
}
</style>
