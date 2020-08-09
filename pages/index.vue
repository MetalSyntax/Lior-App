<template>
  <main
    class="container flex items-center flex-wrap w-full my-0 mx-auto overflow-hidden"
  >
    <form
      class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto h-full"
    >
      <!--Cliente-->
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
          >Ingrese su nombre de cliente</label
        >
        <v-select
          class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 text-sm rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 mb-4 p-2"
          v-model="customer"
          :options="paginated"
          @search="query => (search = query)"
          :filterable="false"
          :clearable="false"
          label="name"
          :value="paginated.name"
          placeholder="NOMBRE DEL CLIENTE"
        ></v-select>
        <button
          class="block bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded cursor-pointer uppercase text-sm my-0 mx-auto"
          @click.prevent="addCustomer"
        >Guardar cliente</button>
      </section>
      <!--Producto-->
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-product"
          >Seleccione un producto</label
        >
        <v-select
          class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 text-sm rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 p-2"
          v-model="productSelected"
          :options="productsData"
          :value="productsData.name"
          :clearable="false"
          label="name"
          placeholder="NOMBRE DEL PRODUCTO"
        ></v-select>
      </section>
      <!--Cantidad-->
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
          >Ingrese la cantidad de los productos</label
        >
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
      <!--Resumen Temporal-->
      <section class="w-full lg:w-full px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
          >Resumen parcial</label
        >
        <div
          class="appearance-none block w-full bg-gray-200 text-gray-500 border rounded py-3 px-4 leading-tight"
        >
          <p class="text-sm uppercase">Precio Unitario:</p>
          <span
            v-if="productSelected.price > 0"
            class="priceProduct text-xl text-gray-700"
            >{{ productSelected.price }}$</span
          >
          <span v-else class="text-md text-gray-700"
            >No hay valor del producto</span
          >
          <br />
          <p class="text-sm uppercase">Precio Subtotal:</p>
          <span
            v-if="productSelected.price > 0"
            class="text-xl text-gray-700"
            >{{ productSelected.price * quantity }}$</span
          >
          <span v-else class="text-md text-gray-700"
            >No hay un producto seleccionado</span
          >
        </div>
      </section>
      <!--Envio-->
      <section class="flex w-full lg:w-full px-3 my-2 lg:my-3 justify-center">
        <button
          class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded cursor-pointer uppercase text-sm"
          @click.prevent="addQuantity"
        >
          Agregar producto
        </button>
      </section>
    </form>
    <!--Resumen-->
    <section
      v-if="allProducts.length > 0"
      class="flex flex-wrap w-full px-2 justify-center"
    >
      <div
        class="appearance-none flex flex-wrap w-full bg-gray-200 text-gray-500 border rounded py-2 px-2 leading-tight"
      >
        <span
          class="block w-full uppercase tracking-wide text-center text-gray-900 text-lg font-bold mb-2"
          >Resumen del Pedido</span
        >
        <span
          class="block w-1/2 uppercase text-center text-gray-900 text-xs font-bold"
        >
          Total:
          <span class="text-2xl">{{ totalPriceFormatted }}$</span>
        </span>
        <span
          class="block w-1/2 uppercase text-center text-gray-900 text-xs font-bold"
        >
          Colecciones:
          <span class="text-2xl">{{ collectionsFormatted }}</span>
        </span>
      </div>
      <span
        class="block uppercase tracking-wide text-gray-900 text-sm font-bold my-3"
        >Listado de productos</span
      >
      <table class="w-full table-auto">
        <thead class="border bg-gray-200">
          <tr>
            <th class="px-2 py-1 text-xs md:text-sm uppercase">Producto</th>
            <th class="px-2 py-1 text-xs md:text-sm uppercase"></th>
          </tr>
        </thead>
        <tbody class="border">
          <tr
            v-for="(allProduct, index) in allProducts"
            v-bind:key="index"
            class="my-2"
          >
            <td class="px-1 py-1 text-left text-xs md:text-sm">
              <span class="w-full block font-bold">
                {{ allProduct.name }}
                </span>
              <span class="w-full uppercase"
                >Unidades: {{ allProduct.quantity }} -</span
              >
              <span class="w-full uppercase"
                >Subtotal: {{ allProduct.subtotal }}$</span
              >
              <!--<button
                class="rounded my-0 mx-auto py-1 px-3 cursor-pointer text-lg text-red-500 font-bold"
                @click="removeEach(index)"
              >
                X
              </button>-->
            </td>
            <td class="text-center">
              <button
                class="rounded my-0 mx-auto py-1 px-3 cursor-pointer text-lg text-red-500 font-bold"
                @click="removeEach(index)"
              >
                <!--<img
                  src="../static/delete.png"
                  alt="Delete"
                  class="w-8 h-auto"
                />-->
                X
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <button
        class="bg-green-500 hover:bg-green-700 text-white my-4 py-2 px-4 rounded cursor-pointer uppercase text-sm"
        @click.prevent="saveArchive"
      >
        Guardar archivo
      </button>
    </section>
  </main>
</template>

<script>
import customersDataJson from "../data/customers.json";
import productsDataJson from "../data/products.json";
import { saveAs } from "file-saver";
const FileSaver = require("file-saver");

export default {
  head() {
    return {
      title: "Lior Cosmetic - Pedidos",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Aplicacion web para generar pedidos de Lior Cosmetic"
        }
      ]
    };
  },
  data() {
    return {
      allProducts: [], // Arreglo de todos los productos
      productSelected: [], // Producto seleccionado por el cliente
      productsSelected: [], // Productos guardados en arreglo
      quantity: null, // Cantidad
      unitPrice: null, // Precio Unitario
      subtotal: null, // Subtotal
      totalPrice: 0, // Precio Total
      totalPriceFormatted: 0, //
      collections: 0, //Precio Total
      collectionsFormatted: 0, //
      productsData: Object.values(productsDataJson), //Data de productos
      file: null, //Data del Archivo
      customer: [], //Cliente
      customerName: "", //Cliente Seleccionado
      customerCode: "", //Cliente guardado
      customersData: Object.values(customersDataJson), //Data de clientes
      search: "",
      offset: 0,
      limit: 3 //Limite de clientes visibles
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
    if (localStorage.getItem("totalPriceFormatted")) {
      this.totalPriceFormatted = JSON.parse(localStorage.getItem("totalPriceFormatted"));
    }
    // Muestra las colecciones
    if (localStorage.getItem("collections")) {
      this.collections = JSON.parse(localStorage.getItem("collections"));
    }
     if (localStorage.getItem("collectionsFormatted")) {
      this.collectionsFormatted = JSON.parse(localStorage.getItem("collectionsFormatted"));
    }
    // Muestra el nombre del cliente
    if (localStorage.getItem("customerName")) {
      this.customerName = JSON.parse(localStorage.getItem("customerName"));
    }
    // Muestra el codigo del cliente
    if (localStorage.getItem("customerCode")) {
      this.customerCode = JSON.parse(localStorage.getItem("customerCode"));
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
      this.totalPriceFormatted = parseFloat(this.totalPrice).toFixed(2);
      this.collectionsFormatted = parseFloat(this.collections).toFixed(2);

      this.allProducts.push({
        name: this.productSelected.name,
        code: this.productSelected.code,
        quantity: parseInt(this.quantity),
        unitedPrice: parseFloat(this.productSelected.price),
        subtotal: parseFloat(this.productSelected.price * this.quantity),
        subtotalformatted: parseFloat(this.productSelected.price * this.quantity).toFixed(2)
      });
      this.productsSelected.push([
        "\n",
        this.productSelected.name,
        this.productSelected.code,
        parseInt(this.quantity),
        this.productSelected.price,
        this.productSelected.price * this.quantity
      ]);
      //Vacia el campo
      this.quantity = null;
      //Guarda la data
      this.saveAll();
    },
    saveAll() {
      const allProducts = JSON.stringify(this.allProducts);
      localStorage.setItem("allProducts", allProducts);
      const productsSelected = JSON.stringify(this.productsSelected);
      localStorage.setItem("productsSelected", productsSelected);
      const totalPrice = JSON.stringify(this.totalPrice);
      localStorage.setItem("totalPrice", totalPrice);
      const collections = JSON.stringify(this.collections);
      localStorage.setItem("collections", collections);
      const totalPriceFormatted = JSON.stringify(this.totalPriceFormatted);
      localStorage.setItem("totalPriceFormatted", totalPriceFormatted);
      const collectionsFormatted = JSON.stringify(this.collectionsFormatted);
      localStorage.setItem("collectionsFormatted", collectionsFormatted);
    },
    removeEach(x) {
      this.totalPrice -= parseFloat(this.allProducts[x].subtotal);
      this.totalPriceFormatted -= parseFloat(this.allProducts[x].subtotal).toFixed(2);
      this.collections = this.totalPrice / 50;
      this.collectionsFormatted = parseFloat(this.totalPrice / 50).toFixed(2);
      this.productsSelected.splice(x, 1);
      this.allProducts.splice(x, 1);
      this.saveAll();
    },
    saveArchive(x) {
      if (
        confirm(
          "Aceptar para Guardar y borrar \n Cancelar para Guardar sin borrar"
        )
      ) {
        this.file = new File(
          [
            "Cliente:\n" + this.customerName,
            "\nCodigo:\n" + this.customerCode,
            "\nProductos:" + this.productsSelected,
            "\nPrecio Total:\n" + parseFloat(this.totalPrice).toFixed(2),
            "\nColecciones:\n" + parseFloat(this.collections).toFixed(2)
          ],
          "Pedido de " + this.customerCode + ".csv",
          { type: "data:text/csv;charset=utf-8,%EF%BB%BF" }
        );
        FileSaver.saveAs(this.file);
        //Borrar Visual
        this.allProducts.splice(x);
        //Borrar LocalStorage
        localStorage.clear();
        //Restablecer Valores
        this.totalPrice = 0;
        this.collections = 0;
        this.quantity = null;
        this.customerName = "";
        this.customerCode = "";
        //Salvar los valores reestrablecidos
        this.saveAll();
      } else {
        this.file = new File(
          [
            "Cliente:\n" + this.customerName,
            "\nCodigo:\n" + this.customerCode,
            "\nProductos:" + this.productsSelected,
            "\nPrecio Total:\n" + parseFloat(this.totalPrice).toFixed(2),
            "\nColecciones:\n" + parseFloat(this.collections).toFixed(2)
          ],
          "Pedido de " + this.customerCode + ".csv",
          { type: "data:text/csv;charset=utf-8,%EF%BB%BF" }
        );
        FileSaver.saveAs(this.file);
      }
    }
  },
  computed: {
    filtered() {
      return this.customersData.filter(customer =>
        customer.name.match(new RegExp(this.search, "gi"))
      );
    },
    paginated() {
      return this.filtered.slice(this.offset, this.limit + this.offset);
    }
  },
  watch: {
    /*customer(customerName, customerCode) {
      localStorage.customerName = this.customer.name;
      localStorage.customerCode = this.customer.code;
    }*/
  }
};
</script>

<style>
.vs__dropdown-toggle {
  border: 0px solid rgba(0, 0, 0, 0);
}
tr:nth-child(even) {
  background-color: #edf2f7;
}
</style>
