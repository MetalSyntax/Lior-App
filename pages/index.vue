<template>
  <main class="container flex flex-wrap items-center content-center self-center w-full my-0 mx-auto overflow-hidden">
    <form class="flex flex-wrap justify-center items-center content-center self-center lg:self-center lg:items-center lg:content-center w-full my-0 mx-auto h-full height-screen">
      <!--<img class="h-40 w-40" src="../assets/img/LC-Logo.png"/>-->
      <h1 class="text-center block w-full text-6xl h-20 text-brown-700">LIOR</h1>
      <h2 class="text-center block w-full text-lg pb-6 text-brown-700">COSMETICS</h2>
      <Customers />
    </form>
    <footer class="w-full text-center text-xs uppercase bottom-0 p-1 text-brown-800">
      <p class="">Copyright © 2020. Created by <a href="https://metalsyntax.now.sh/" target="_blank" class="text-brown-800 font-bold">MetalSyntax</a> for <a href="http://www.liorcosmetics.com/" target="_blank" class="text-brown-800 font-bold">Lior Cosmetics</a>.</p>
    </footer>
  </main>
</template>

<script>
export default {
  head() {
    return {
      title: "Lior Cosmetics - Pedidos",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Aplicación web para generar pedidos en Lior Cosmetics",
        },
      ],
    };
  },
  data() {
    return {};
  },
  mounted() {},
  methods: {},
  computed: {},
  watch: {
    /*customer(customerName, customerCode) {
      localStorage.customerName = this.customer.name;
      localStorage.customerCode = this.customer.code;
    }*/
  },
};
</script>

<style>
.height-screen {
  height: 90vh;
}
/*.color-blue-custom {
   color: #244a8b;
}*/
</style>
