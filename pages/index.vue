<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto overflow-hidden">
    <form
      v-on:submit.prevent="addQuantity"
      class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto"
    >
      <h1
        v-if="customer"
        class="block w-full text-gray-900 text-center text-xl bold py-2"
      >Hola!, {{ customer }}</h1>
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

      <table class="w-full table-auto">
        <thead>
          <tr>
            <th class="px-2 py-1 text-sm">Nombre</th>
            <th class="px-2 py-1 text-sm">Cantidad</th>
            <!--<th class="px-2 py-1 text-sm">Unitario</th>-->
            <th class="px-2 py-1 text-sm">Total</th>
            <th class="px-2 py-1 text-sm">Borrar</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(allProduct, index) in allProducts" v-bind:key="index">
            <td class="border px-1 py-1 text-center text-sm md:text-base">
              Pre - Coco 240ML
              <!--{{allProduct[index].productSelected}}-->
            </td>
            <td class="border px-4 py-2 text-center text-sm md:text-base">{{allProduct[1]}}</td>
            <!--<td class="border px-4 py-2 text-center text-sm md:text-base">{{allProduct[2]}}$</td>-->
            <td class="border px-4 py-2 text-center text-sm md:text-base">{{allProduct[3]}}$</td>
            <td class="border text-center px-4 py-2">
              <button
                class="bg-red-500 hover:bg-red-700 text-white rounded my-0 mx-auto py-1 px-2"
                @click="removeEach(index)"
              >X</button>
            </td>
          </tr>
        </tbody>
      </table>
      <!--<section
        v-for="(allProduct, index) in allProducts"
        v-bind:key="index"
        class="w-full flex flex-wrap border-gray-500 border-solid py-2 border-b-2 mx-2 justify-between"
      >
        <span class="w-full md:w-5/6 text-ms">Cantidad: {{allProduct[index].quantitySelected}}</span>
        <span
          class="w-full md:w-5/6 text-ms"
        >Precio Unitario: {{allProduct[index].unitPriceSelected}}</span>
        <span
          class="w-full md:w-5/6 text-ms"
        >Precio total parcial: {{allProduct[index].totalPartialPrice}}</span>
        <button
          class="bg-red-500 hover:bg-red-700 text-white rounded my-0 mx-auto px-1"
          @click="removeEach(index)"
        >Borrar</button>
      </section>-->
    </section>
  </main>
</template>

<script>
/*import customersData from '../data/customers.json'
import productsData from '../data/products.json'*/
//.join(' \r\n')
export default {
  data() {
    return {
      /*customers: customersData,
      products: productsData,*/
      allProducts: [],
      customer: null,
      quantity: null,

      /*productSelected: [],
      quantitySelected: [],
      unitPriceSelected: [],
      totalPartialPrice: [],*/

      productsSelected: null,
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
      } catch (e) {
        localStorage.removeItem("allProducts");
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
          this.productsData.name,
          parseFloat(this.quantity),
          this.productsData.price,
          this.productsData.price * this.quantity,
        ])
      );
      this.quantity = null;

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
    },
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
