<template>
  <main class="container flex flex-wrap items-center content-center self-center w-full my-0 mx-auto overflow-hidden height-screen">
    <form class="flex flex-wrap justify-center items-center content-center self-center py-4 w-full max-w-8xl my-0 mx-auto h-full">
      <img class="h-64 w-full" src="../assets/img/LC-Logo.png"/>
      <Customers />
    </form>
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
  height: 85vh;
}
</style>
