<template>
  <main class="container flex flex-wrap items-center content-center self-center w-full my-0 mx-auto overflow-hidden">
    <form class="flex flex-wrap justify-center items-center content-center self-center lg:self-center lg:items-center lg:content-center w-full my-0 mx-auto h-full height-screen">
      <!--<img class="h-40 w-40" src="../assets/img/LC-Logo.png"/>-->
      <h1 class="text-center block w-full text-6xl font-semibold h-20 text-brown-800">LIOR</h1>
      <h2 class="text-center block w-full text-lg font-semibold pb-6 text-brown-800">COSMETICS</h2>
      <Customers />
    </form>
    <footer class="w-full text-center text-xs uppercase bottom-0 p-1 text-brown-800">
      <p class="">Copyright © 2020. Created by <a href="https://metalsyntax.now.sh/" target="_blank" class="text-brown-800 font-bold">MetalSyntax</a>.</p>
    </footer>
  </main>
</template>

<script>
export default {
  head() {
    return {
      title: "Lior Cosmetic - Pedidos",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Aplicacion web para generar pedidos de Lior Cosmetic",
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
