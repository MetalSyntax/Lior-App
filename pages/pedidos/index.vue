<template>
  <main
    class="flex flex-wrap items-center content-center self-center md:w-10/12 xl:w-full w-full my-0 mx-auto overflow-hidden"
  >
    <form
      class="flex flex-wrap justify-center items-center content-center self-center xl:self-center xl:items-center xl:content-center my-0 form-height xl:h-full w-full xl:w-7/12 lg:mt-0 px-0 xl:px-10"
    >
      <h1
        class="text-center block w-full text-5xl lg:text-6xl text-brown-700 mt-6 z-10 bg-white leading-none"
      >
        LIOR
      </h1>
     <h2
        class="text-center block w-full text-lg pb-6 text-brown-700 z-10 bg-white"
      >
        PEDIDOS
      </h2>
      <Customers
        class="bg-white"
      />
      <a class="mt-10 text-xs text-center uppercase" href="https://wa.me/584122311303?text=No%20puedo%20ingresar%20a%20la%20plataforma%20de%20pedidos.">¿Tienes problemas para ingresar pedidos? ¡Escríbenos!</a>
    </form>
    <img class="w-5/12 image-height hidden xl:flex" src="../../static/img-index.jpg">
    <footer
      class="relative z-0 left-0 w-full text-center text-xs uppercase bottom-0 p-1 text-brown-800 content-end self-end"
    >
      <p class>
        Todos los derechos reservados © 2020. Creado por
        <a
          href="https://metalsyntax.now.sh/"
          target="_blank"
          class="text-brown-800 font-bold"
          rel="noreferrer noopener"
          >MetalSyntax</a
        >
        para
        <a
          href="http://www.liorcosmetics.com/"
          target="_blank"
          class="text-brown-800 font-bold"
          rel="noreferrer noopener"
          >Lior Cosmetics</a
        >.
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
.image-height {
  height: calc(100vh - 96px);
}
.form-height {
  height: calc(100vh + 81px);
}
@media (min-width: 1024px) {
  .form-height {
    height: calc(100vh - 96px);
  }
}
/*.index {
  height: 100vh;
}
@media (min-width: 1024px) {
  .index {
    height: 80vh;
    background: linear-gradient(to top, transparent 50%, #604728 50%);
  }
}*/
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
