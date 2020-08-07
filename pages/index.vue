<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto overflow-hidden">
    <form class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto h-full">
      <section class="flex flex-wrap w-full px-3 my-2 lg:my-3">
        <h1
          v-if="customerName.length > 0"
          class="block w-full text-gray-900 text-center text-lg bold py-2 uppercase"
        >
          Bienvenido!,
          <span class="block w-full">{{ customerName }}</span>
        </h1>
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
        >Ingrese su nombre de cliente</label>
        <v-select
          class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 text-sm rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 mb-4"
          v-model="customer"
          :options="paginated"
          @search="query => (search = query)"
          :filterable="false"
          label="name"
          :value="paginated.name"
          placeholder="NOMBRE DEL CLIENTE"
        ></v-select>
        <button
          class="block bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded cursor-pointer uppercase text-sm my-0 mx-auto"
          @click.prevent="addCustomer"
        >Guardar cliente</button>
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
          placeholder="NOMBRE DEL PRODUCTO"
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
          placeholder="CANTIDAD"
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
          <p class="text-sm uppercase">Precio Unitario:</p>
          <span
            v-if="productSelected.price > 0"
            class="priceProduct text-xl text-gray-700"
          >{{ productSelected.price }}$</span>
          <span v-else class="priceProduct text-md text-gray-700">No hay valor del producto</span>
          <br />
          <p class="text-sm uppercase">Precio Subtotal:</p>
          <span
            v-if="productSelected.price > 0"
            class="totalProducts text-xl text-gray-700"
          >{{(productSelected.price * quantity).toFixed(2)}}$</span>
          <span v-else class="totalProducts text-md text-gray-700">No hay un producto seleccionado</span>
        </div>
      </section>
      <section class="flex w-full lg:w-full px-3 my-2 lg:my-3 justify-center">
        <button
          class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded cursor-pointer uppercase text-sm"
          @click.prevent="addQuantity"
        >Agregar producto</button>
      </section>
    </form>

    <section v-if="allProducts.length > 0" class="flex flex-wrap w-full px-2 justify-center">
      <div
        class="appearance-none flex flex-wrap w-full bg-gray-200 text-gray-500 border rounded py-2 px-2 leading-tight"
      >
        <span
          class="block w-full uppercase tracking-wide text-center text-gray-900 text-xl font-bold mb-2"
        >Resumen del Pedido</span>
        <span class="block w-1/2 uppercase text-center text-gray-900 text-xs font-bold">
          Total:
          <span class="text-2xl">{{totalPrice.toFixed(2)}}$</span>
        </span>
        <span class="block w-1/2 uppercase text-center text-gray-900 text-xs font-bold">
          Colecciones:
          <span class="text-2xl">{{collections.toFixed(2)}}</span>
        </span>
      </div>
      <span
        class="block uppercase tracking-wide text-gray-900 text-sm font-bold my-3"
      >Listado de productos seleccionados</span>
      <table class="w-full table-auto">
        <thead class="border bg-gray-200">
          <tr>
            <th class="px-2 py-1 text-sm uppercase">Producto</th>
            <th class="px-2 py-1 text-sm uppercase">Borrar</th>
          </tr>
        </thead>
        <tbody class="border">
          <tr v-for="(allProduct, index) in allProducts" v-bind:key="index" class="my-2">
            <td class="px-1 py-1 text-center text-sm md:text-base">
              <span class="w-full block">{{allProduct[1]}}</span>
              <span class="w-full uppercase">Unidades: {{allProduct[3]}} -</span>
              <span class="w-full uppercase">Subtotal: {{allProduct[5].toFixed(2)}}$</span>
            </td>
            <td class="text-center">
              <button
                class="text-white rounded my-0 mx-auto py-1 px-3 cursor-pointer"
                @click="removeEach(index)"
              >
                <img src="../static/delete.png" alt="Delete" class="w-8 h-auto" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <button
        class="bg-green-500 hover:bg-green-700 text-white my-4 py-2 px-4 rounded cursor-pointer uppercase text-sm"
        @click.prevent="saveArchive"
      >Guardar archivo</button>
    </section>
  </main>
</template>

<script>
import customersDataJson from "../data/customers.json";
import productsDataJson from "../data/products.json";
import { saveAs } from "file-saver";
const FileSaver = require("file-saver");

export default {
  data() {
    return {
      allProducts: [], // Arreglo de todos los productos
      productSelected: [], // Producto seleccionado por el cliente
      productsSelected: null, // Productos guardados en arreglo
      quantity: null, // Cantidad
      unitPrice: null, // Precio Unitario
      totalPrice: 0, // Precio Total
      collections: 0, //Precio Total
      productsData: Object.values(productsDataJson), //Data de productos
      file: null, //
      customer: [], //Cliente
      customerName: "", //Cliente Seleccionado
      customerCode: "", //Cliente guardado
      customersData: Object.values(customersDataJson), //Data de clientes
      search: "",
      offset: 0,
      limit: 3, //Limite de clientes visibles
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
    // Guarda el nombre del cliente
    if (localStorage.getItem("customerName")) {
      this.customerName = JSON.parse(localStorage.getItem("customerName"));
    }
  },
  methods: {
    addCustomer() {
      (this.customerCode = this.customer.code),
        (this.customerName = this.customer.name);
      this.saveCustomer();
    },
    saveCustomer() {
      const customerName = JSON.stringify(this.customerName);
      localStorage.setItem("customerName", customerName);
      const customerCode = JSON.stringify(this.customerCode);
      localStorage.setItem("customerCode", customerCode);
    },
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
      this.totalPrice += this.productSelected.price * this.quantity;
      this.collections = this.totalPrice / 50;
      this.allProducts.push(
        (this.productsSelected = [
          "\n",
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
      this.totalPrice -= this.allProducts[x][5];
      this.collections = this.totalPrice / 50;
      this.allProducts.splice(x, 1);
      this.saveAll();
    },
    saveArchive(x) {
      if (
        confirm("Ok para Guardar y borrar \n Cancelar para Guarda sin borrar")
      ) {
        this.file = new File(
          [
            "Cliente:\n" + this.customerName,
            "\nCodigo:\n" + this.customerCode,
            "\nProductos:" + this.allProducts,
            "\nPrecio Total:\n" + this.totalPrice,
            "\nColecciones:\n" + this.collections,
          ],
          "Pedido.csv",
          { type: "data:text/csv;charset=utf-8" }
        );
        FileSaver.saveAs(this.file);
        //Borrar Visual
        this.allProducts.splice(x);
        //Borrar LocalStorage
        localStorage.clear();
        //Restablecer Valores
        this.totalPrice = 0;
        this.collections= 0;
        this.quantity = null;
        //Salvar los valores reestrablecidos
        this.saveAll();
      } else {
        this.file = new File(
          [
            "Cliente:\n" + this.customerName,
            "\nCodigo:\n" + this.customerCode,
            "\nProductos:" + this.allProducts,
            "\nPrecio Total:\n" + this.totalPrice,
            "\nColecciones:\n" + this.collections,
          ],
          "Pedido de " + this.customerCode + ".csv",
          { type: "data:text/csv;charset=utf-8" }
        );
        FileSaver.saveAs(this.file);
      }
    },
  },
  computed: {
    filtered() {
      return this.customersData.filter((customer) =>
        customer.name.match(new RegExp(this.search, "gi"))
      );
    },
    paginated() {
      return this.filtered.slice(this.offset, this.limit + this.offset);
    },
  },
  watch: {},
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
tr:nth-child(even) {
  background-color: #edf2f7;
}
</style>
