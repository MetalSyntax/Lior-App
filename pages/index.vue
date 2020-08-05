<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto overflow-hidden">
    <form
      v-on:submit.prevent="addQuantity"
      class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto h-full"
    >
      <h1
        v-if="customer.name != null"
        class="block w-full text-gray-900 text-center text-xl bold py-2 customer"
      >Hola!, {{ customer.name }}</h1>
      <section class="w-full lg:w-full px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
        >Ingrese su codigo de cliente</label>
        <v-select
          class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 text-sm rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
          v-model="customer"
          :options="paginated"
          @search="query => search = query"
          :filterable="false"
          label="name"
          :value="paginated.name"
          placeholder="Nombre del cliente"
        ></v-select>
      </section>
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-product"
        >Seleccione un producto</label>
        <v-select
          class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 text-sm rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
          v-model="productSelected"
          :options="productsData"
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
          <p>Precio Unitario:</p>
          <span
            v-if="productSelected.price > 0"
            class="priceProduct text-lg text-gray-700"
          >{{ productSelected.price }}$</span>
          <span v-else class="priceProduct text-gray-700">No hay valor del producto</span>
          <br />
          <p>Precio Subtotal:</p>
          <span
            v-if="productSelected.price > 0"
            class="totalProducts text-lg text-gray-700"
          >{{productSelected.price * quantity}}$</span>
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

    <section v-if="allProducts.length > 0" class="flex flex-wrap w-full px-2 justify-center">
      <div
        class="appearance-none block w-full bg-gray-200 text-gray-500 border rounded py-2 px-2 leading-tight"
      >
        <span
          class="block w-full uppercase tracking-wide text-center text-gray-900 text-lg font-bold mb-2"
        >Pedido / Colecciones</span>
        <span
          class="block w-full uppercase tracking-wide text-center text-gray-900 text-3xl font-bold"
        >{{totalPrice}}$ / {{collections}}</span>
      </div>
      <span
        class="block uppercase tracking-wide text-gray-900 text-sm font-bold my-3"
      >Listado de productos seleccionados</span>
      <table class="w-full table-auto">
        <thead class="border bg-gray-200">
          <tr>
            <th class="px-2 py-1 text-sm">Nombre</th>
            <th class="px-2 py-1 text-sm">Cantidad</th>
            <!--<th class="px-2 py-1 text-sm">Unitario</th>-->
            <th class="px-2 py-1 text-sm">Subtotal</th>
            <th class="px-2 py-1 text-sm">Borrar</th>
          </tr>
        </thead>
        <tbody class="border">
          <tr v-for="(allProduct, index) in allProducts" v-bind:key="index" class="my-2">
            <td class="px-1 py-1 text-center text-sm md:text-base">
              <span class="w-full">{{allProduct[0].toLowerCase()}}</span>
              <!--{{allProduct[index].productSelected}}-->
            </td>
            <td class="px-4 py-2 text-center text-sm md:text-base">{{allProduct[2]}}</td>
            <!--<td class="border px-4 py-2 text-center text-sm md:text-base">{{allProduct[2]}}$</td>-->
            <td class="px-4 py-2 text-center text-sm md:text-base">{{allProduct[4]}}$</td>
            <td class="text-center">
              <button class="text-white rounded my-0 mx-auto py-1 px-3" @click="removeEach(index)">
                <img src="../static/delete.png" alt="Delete" class="w-8 h-auto" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </main>
</template>

<script>
import customersDataJson from "../data/customers.json";
import productsDataJson from "../data/products.json";

export default {
  data() {
    return {
      allProducts: [], // Arreglo de todos los productos
      productSelected: [], // Producto seleccionado por el cliente
      productsSelected: null, // Productos guardados en arreglo
      customer: [], //Cliente
      customerCurrent: null, //Cliente guardado
      quantity: null, // Cantidad
      unitPrice: null, // Precio Unitario
      totalPrice: 0, // Precio Total
      collections: 0, //Precio Total
      productsData: Object.values(productsDataJson), //Data de productos
      customersData: Object.values(customersDataJson), //Data de clientes
      search: '',
      offset: 0,
      limit: 3,
    };
  },
  mounted() {
    //Muestra los datos seleccionados de los productos
    if (localStorage.getItem("allProducts")) {
      try {
        this.allProducts = JSON.parse(localStorage.getItem("allProducts"));
      } catch (e) {
        localStorage.removeItem("allProducts");
      }
    }
    // Muestra el precio total
    if (localStorage.getItem("totalPrice")) {
      this.totalPrice = JSON.parse(localStorage.getItem("totalPrice"));
    }
    // Muestra las colecciones
    if (localStorage.getItem("collections")) {
      this.collections = JSON.parse(localStorage.getItem("collections"));
    }
    //Muestra el nombre del cliente
    /*if (localStorage.customerCurrent) {
      this.customer = localStorage.customerCurrent;
    }*/
  },
  methods: {
    addQuantity() {
      if (!this.productSelected.price) {
        return;
      }
      if (!this.productSelected.code) {
        return;
      }
      if (!this.productSelected.name) {
        return;
      }
      if (!this.quantity) {
        return;
      }
      //localStorage.customerCurrent = this.customer;
      this.totalPrice += this.productSelected.price * this.quantity;
      this.collections = this.totalPrice / 50;
      this.allProducts.push(
        (this.productsSelected = [
          this.productSelected.name, //Nombre
          this.productSelected.code, //Codigo
          parseFloat(this.quantity), //Cantidad
          this.productSelected.price, //Precio Unitario
          this.productSelected.price * this.quantity, //Subtotal
        ])
      );
      //Vacia el campo
      this.quantity = null;
      //Guarda la data
      this.saveAll();
    },
    saveAll() {
      const storageProducts = JSON.stringify(this.allProducts);
      localStorage.setItem("allProducts", storageProducts);
      const totalPrice = JSON.stringify(this.totalPrice);
      localStorage.setItem("totalPrice", totalPrice);
      const collections = JSON.stringify(this.collections);
      localStorage.setItem("collections", collections);
    },
    removeEach(x) {
      this.totalPrice -= this.allProducts[x][4];
      this.collections = this.totalPrice / 50;
      this.allProducts.splice(x, 1);
      this.saveAll();
    },
  },
  computed: {
    filtered() {
      return this.customersData.filter(customer => customer.name.match(new RegExp(this.search,"gi")));
    },
    paginated() {
      return this.filtered.slice(this.offset, this.limit + this.offset);
    },
  },
  watch: {
    //Guarda temporalmente el nombre del cliente
    customer(customerCurrent) {
      localStorage.customerCurrent = JSON.stringify(this.customer);
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
.vs__clear {
  display: none;
}
.customer {
  text-transform: lowercase;
}
.customer:first-letter {
  text-transform: capitalize;
}
tr:nth-child(even) {
  background-color: #edf2f7;
}
</style>
