<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto">
    <h1 v-if="customer" class="block w-full text-gray-900 text-center text-xl bold py-2">Hola!, {{ customer }}</h1>
    <form
      v-on:submit.prevent="addQuantity"
      class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto"
    >
      <section class="w-full lg:w-full px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
        >Ingrese su codigo de cliente</label>
        <input
          class="appearance-none block w-full bg-gray-200 text-gray-900 border rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
          id="grid-code"
          name="customer"
          type="text"
          maxlength="7"
          placeholder="Codigo del cliente"
          v-model="customer"
        />
      </section>
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-product"
        >Seleccione un producto</label>
        <v-select
          class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
          v-model="productsData.price"
          :options="productsData"
          :reduce="productsData => productsData.price"
          :value="productsData.name"
          label="name"
          placeholder="Nombre del producto"
        ></v-select>
      </section>
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
        >Ingrese la cantidad de los productos</label>
        <input
          class="appearance-none block w-full bg-gray-200 text-gray-900 border rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
          id="grid-quantity"
          type="number"
          placeholder="Cantidad"
          maxlength="4"
          min="1"
          max="9999"
          value="0"
          v-model="quantity"
        />
      </section>
      <section class="w-full lg:w-full px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
        >Resumen parcial</label>
        <div
          class="appearance-none block w-full bg-gray-200 text-gray-500 border rounded py-3 px-4 leading-tight"
        >
          <p>Precio unitario:</p>
          <span
            v-if="productsData.price > 0"
            class="priceProduct text-lg text-gray-700"
          >{{ productsData.price }}$</span>
          <span v-else class="priceProduct text-gray-700">No hay valor del producto</span>
          <br />
          <p>Precio Total:</p>
          <span
            v-if="productsData.price > 0"
            class="totalProducts text-lg text-gray-700"
          >{{productsData.price * quantity}}$</span>
          <span v-else class="totalProducts text-gray-700">No hay un producto seleccionado</span>
        </div>
      </section>
      <section class="flex w-full lg:w-full px-3 my-2 lg:my-3 justify-center">
        <input
          class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded"
          type="submit"
          value="Agregar producto"
        />
      </section>
    </form>
    <section class="flex flex-wrap w-full px-2 my-2 lg:my-3 justify-center">
      <span
        class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
      >Listado de productos seleccionados</span>
      <section
        v-for="(allProduct, index) in allProducts"
        v-bind:key="index"
        class="w-full flex flex-wrap border-gray-500 border-solid py-2 border-b-2 mx-2 md:justify-between"
      >
        <p class="w-full md:w-5/6 text-ms my-2">{{allProduct.join(' \r\n')}}</p>
        <button class="w-4/6 md:w-1/6 bg-red-500 hover:bg-red-700 text-white py-2 px-4 rounded my-0 mx-auto" @click="removeEach(index)">Borrar</button>
      </section>
    </section>
  </main>
</template>

<script>
/*import customersData from '../data/customers.json'
import productsData from '../data/products.json'*/

export default {
  data() {
    return {
      /*customers: customersData,
      products: productsData,*/
      allProducts: [],
      customer: null,
      quantity: null,
      quantitySelected: null,
      productsSelected: null,
      productSelected: null,
      customerCurrent: null,
      unitPrice: null,
      totalPrice: 0,
      productsData: [
        {
          code: "EX-0001",
          name: "Pre - Coco 240ML",
          price: 2,
        },
        {
          code: "EX-0002",
          name: "Post - Coco 240 ML",
          price: 3,
        },
        {
          code: "EX-0003",
          name: "Pre - Cayena 240ML",
          price: 4,
        },
        {
          code: "EX-0004",
          name: "Post - Cayena 240ML",
          price: 5,
        },
      ],
      customersData: [
        {
          code: "EA-0001",
          name: "Wonder",
        },
        {
          code: "EA-0002",
          name: "Jhonny",
        },
      ],
    };
  },
  mounted() {
    if (localStorage.getItem("allProducts")) {
      try {
      this.allProducts = JSON.parse(localStorage.getItem("allProducts"));
      } catch(e) {
        localStorage.removeItem('allProducts');
      }
    }
    if (localStorage.customerCurrent) {
      this.customer = localStorage.customerCurrent;
    }
  },
  methods: {
    addQuantity() {
      //localStorage.customerCurrent = this.customer;

      this.totalPrice += this.productsData.price * this.quantity;
      localStorage.totalPrice = this.totalPrice;

      this.allProducts.push(
        (this.productsSelected = [
          "Nombre del Producto: " + this.productsData.name,
          "Cantidad: " + this.quantity,
          "Precio Unitario: " + this.productsData.price,
          "Precio total parcial: " + this.productsData.price * this.quantity,
        ])
      );

      this.quantity = "";

      this.saveAll();
    },
    saveAll() {
      const parsed = JSON.stringify(this.allProducts);
      localStorage.setItem("allProducts", parsed);
    },
    removeEach(x) {
      this.allProducts.splice(x, 1);
      this.saveAll();
    },
  },
  computed: {},
  watch: {
    customer(customerCurrent) {
      localStorage.customerCurrent = this.customer;
    }
  },
};
</script>

<style>
.vs__dropdown-toggle {
  border: 0px solid rgba(0, 0, 0, 0);
}
.vs__search,
.vs__search:focus {
  padding: 7px;
}
</style>
