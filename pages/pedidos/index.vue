<template>
  <main
    class="index flex flex-wrap items-center content-center self-center md:w-10/12 lg:w-full lg:px-48 xl:px-64 w-full my-0 mx-auto overflow-hidden"
  >
    <form
      class="flex flex-wrap justify-center md:self-center md:items-center md:content-center items-center content-start self-center w-full my-0 mx-auto h-full"
    >
      <!--<img class="h-40 w-40" src="../assets/img/LC-Logo.png"/>-->
      <h1 class="text-center block w-full text-6xl h-20 text-brown-700 mt-10 z-10 lg:border-t lg:border-r lg:border-l lg:border-brown-400 bg-white">LIOR</h1>
      <h2 class="text-center block w-full text-lg pb-6 text-brown-700 z-10 lg:border-r lg:border-l lg:border-brown-400 bg-white">COSMETICS</h2>
      <Customers class="lg:border-r lg:border-l lg:border-b lg:border-brown-400 bg-white"/>
    </form>
    <footer
      class="absolute left-0 w-full text-center text-xs uppercase bottom-0 p-1 text-brown-800 content-end self-end"
    >
      <p class>
        Todos los derechos reservados © 2020. Creado por
        <a
          href="https://metalsyntax.now.sh/"
          target="_blank"
          class="text-brown-800 font-bold"
          rel="noreferrer noopener"
        >MetalSyntax</a> para
        <a
          href="http://www.liorcosmetics.com/"
          target="_blank"
          class="text-brown-800 font-bold"
          rel="noreferrer noopener"
        >Lior Cosmetics</a>.
      </p>
    </footer>
  </main>
</template>

<script>
export default {
  head() {
    return {
      title: "Lior Cosmetics | Pedidos",
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
.index {
  height: 80vh;
}
@media (min-width: 1024px) {
  .index {
    background: linear-gradient(to top, transparent 50%, #604728 50%);
  }
}

.fade-in {
  animation: fadeIn linear 1.5s;
  -webkit-animation: fadeIn linear 1.5s;
  -moz-animation: fadeIn linear 1.5s;
  -o-animation: fadeIn linear 1.5s;
  -ms-animation: fadeIn linear 1.5s;
}
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-moz-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-webkit-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-o-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-ms-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
