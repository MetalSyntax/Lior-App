<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto overflow-hidden">
    <h1 class="block w-full text-gray-900 text-center text-lg bold py-2 uppercase">
      Bienvenido!,
      <span class="block w-full font-bold">{{ customerName }}</span>
    </h1>
    <form class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto h-full">
      <!--Producto-->
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-product"
        >Seleccione un producto</label>
        <v-select
          class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 text-xs rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 mb-4 p-2"
          v-model="productSelected"
          :options="paginated"
          @search="query => (search = query)"
          :filterable="false"
          :value="paginated.name"
          :clearable="false"
          :selectable="option => option.value != false"
          label="name"
          placeholder="INGRESE NOMBRE"
        >
          <li slot="list-footer" class="flex">
            <button
              class="flex-grow hover:pointer"
              @click.prevent="offset -= 5"
              :disabled="!hasPrevPage"
            >Previo</button>
            <button
              class="flex-grow hover:pointer"
              @click.prevent="offset += 5"
              :disabled="!hasNextPage"
            >Siguiente</button>
          </li>
        </v-select>
      </section>
      <!--Cantidad-->
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
        >Ingrese cantidad</label>
        <input
          class="appearance-none block w-full bg-gray-200 text-gray-900 text-sm border rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
          id="grid-quantity"
          type="number"
          placeholder="0"
          maxlength="4"
          min="1"
          max="9999"
          value="0"
          v-model="quantity"
        />
      </section>
      <!--Resumen Temporal-->
      <section class="w-full lg:w-full px-3 mt-2 mb-16 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-code"
        >Resumen parcial</label>
        <div
          class="appearance-none flex w-full bg-gray-200 text-gray-500 border rounded py-3 px-4 leading-tight"
        >
          <div class="flex flex-wrap w-1/2">
            <p class="text-sm uppercase">Precio Unitario:</p>
            <span
              v-if="productSelected.price > 0"
              class="text-xl text-gray-700"
            >{{ unitPriceFormat }}$</span>
            <span v-else class="text-md text-gray-700"></span>
          </div>
          <div class="flex flex-wrap w-1/2">
            <p class="text-sm uppercase">Precio Subtotal:</p>
            <span
              v-if="productSelected.price > 0"
              class="text-xl text-gray-700"
            >{{ subtotalFormat }}$</span>
            <span v-else class="text-md text-gray-700"></span>
          </div>
        </div>
      </section>
    </form>
    <!--Resumen-->
    <section v-if="allProducts.length > 0" class="flex flex-wrap w-full px-2 justify-center pb-20">
      <div
        class="appearance-none flex flex-wrap w-full bg-white text-gray-500 border rounded py-2 px-2 leading-tight shadow-sm"
      >
        <span
          class="block w-full uppercase tracking-wide text-center color-button-blue text-lg font-bold mb-2"
        >Resumen del Pedido</span>
        <div class="block w-1/2 text-center">
          <span class="uppercase text-gray-900 text-xs font-bold">
            Total:
            <span class="text-2xl">{{ totalPriceFormatted }}$</span>
          </span>
        </div>
        <div class="block w-1/2 text-center">
          <span class="uppercase text-gray-900 text-xs font-bold">
            Colecciones:
            <span class="text-2xl">{{ collectionsFormatted }}</span>
          </span>
        </div>
      </div>
      <span
        class="block uppercase tracking-wide text-gray-900 text-sm font-bold my-3"
      >Listado de productos</span>
      <table class="w-full table-auto">
        <thead class="border bg-gray-200">
          <tr>
            <th class="px-2 py-1 text-xs md:text-sm uppercase text-left">Producto</th>
            <th class="px-2 py-1 text-xs md:text-sm uppercase"></th>
          </tr>
        </thead>
        <tbody class="border">
          <tr v-for="(allProduct, index) in allProducts" v-bind:key="index" class="my-2">
            <td class="px-1 py-1 text-left text-xs md:text-sm">
              <span class="w-full block font-bold">{{ allProduct.name }}</span>
              <span class="w-full uppercase">Unidades: {{ allProduct.quantity }} -</span>
              <span class="w-full uppercase">Subtotal: {{ allProduct.subtotalformatted }}$</span>
            </td>
            <td class="text-center">
              <button
                class="rounded my-0 mx-auto py-1 px-3 cursor-pointer text-lg text-red-500 hover:text-red-700 font-bold"
                @click="removeEach(index)"
              >X</button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
    <!--Botones-->
    <section
      class="flex flex-wrap w-full px-2 justify-center fixed bottom-0 left-0 bg-white mx-auto my-0 max-w-8xl space-between border-t border-gray-200"
    >
      <button
        class="bg-white color-button-green border-button-green hover:border-transparent border mx-2 my-2 py-2 px-4 rounded cursor-pointer uppercase text-sm focus:outline-none hove:outline-none"
        @click.prevent="saveArchive"
      >Guardar</button>
      <button
        class="bg-button text-white border-button border-white hover:border-transparent border mx-2 my-2 py-2 px-8 rounded cursor-pointer uppercase text-sm focus:outline-none hove:outline-none"
        @click.prevent="addQuantity"
      >Agregar</button>
    </section>
  </main>
</template>

<script>
import productsDataJson from "../data/products.json";
import { saveAs } from "file-saver";
const FileSaver = require("file-saver");

export default {
  data() {
    return {
      allProducts: [], // Arreglo de todos los productos
      productSelected: [], // Producto seleccionado por el cliente
      productsSelected: [], // Productos guardados en arreglo
      productsData: Object.values(productsDataJson), //Data de productos
      customerName: null,
      unitPrice: null, // Precio Unitario
      subtotal: null, // Subtotal
      totalPrice: 0, // Precio Total
      totalPriceFormatted: 0, //
      collections: 0, //Precio Total
      collectionsFormatted: 0, //
      quantity: null, // Cantidad
      file: null, //Data del Archivo
      search: "",
      offset: 0,
      limit: 5, //Limite de clientes visibles
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
      this.totalPriceFormatted = JSON.parse(
        localStorage.getItem("totalPriceFormatted")
      );
    }
    // Muestra las colecciones
    if (localStorage.getItem("collections")) {
      this.collections = JSON.parse(localStorage.getItem("collections"));
    }
    if (localStorage.getItem("collectionsFormatted")) {
      this.collectionsFormatted = JSON.parse(
        localStorage.getItem("collectionsFormatted")
      );
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
        //value: this.productSelected.value = false,
        quantity: parseInt(this.quantity),
        unitedPrice: parseFloat(this.productSelected.price),
        subtotal: parseFloat(this.productSelected.price * this.quantity),
        subtotalformatted: parseFloat(
          this.productSelected.price * this.quantity
        ).toFixed(2),
      });
      this.productsSelected.push([
        "\n",
        this.productSelected.name,
        this.productSelected.code,
        parseInt(this.quantity),
        parseFloat(this.productSelected.price).toFixed(2),
        parseFloat(this.productSelected.price * this.quantity).toFixed(2),
      ]);
      //Vacia el campo
      this.quantity = null;
      //Guarda la data
      this.saveAll();
      //console.log(this.productsData);
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
      //this.productsData[x].value = true
      //this.allProducts[x].value = true;
      //this.productsSelected[x].value = true;
      this.totalPriceFormatted -= parseFloat(
        this.allProducts[x].subtotal
      ).toFixed(2);
      this.collections = this.totalPrice / 50;
      this.collectionsFormatted = parseFloat(this.totalPrice / 50).toFixed(2);
      this.productsSelected.splice(x, 1);
      this.allProducts.splice(x, 1);
      this.saveAll();
      //console.log(this.productsData);
    },
    saveArchive(x) {
      if (confirm("¿Deseas guardar y borrar?")) {
        this.file = new File(
          [
            ",Cliente\n," + this.customerName,
            "\n,Código\n," + this.customerCode,
            "\n,Productos,Código,Cantidad,Precio Unitario,Subtotal" +
              this.productsSelected,
            "\n,Precio Total\n," + parseFloat(this.totalPrice).toFixed(2),
            "\n,Colecciones\n," + parseFloat(this.collections).toFixed(2),
          ],
          "Pedido de " +
            this.customerCode +
            " del " +
            new Date().getDate() +
            "-" +
            new Date().getMonth() +
            "-" +
            new Date().getFullYear() +
            ".txt",
          {
            type: "text/plain;charset=utf-8",
            //type: "data:text/csv;charset=utf-8,%EF%BB%BF"
          }
        );
        FileSaver.saveAs(this.file);
        //Borrar Visual
        this.allProducts.splice(x);
        //Restablecer Valores
        this.totalPrice = 0;
        this.collections = 0;
        this.quantity = null;
        this.productsSelected = [];
        this.collectionsFormatted = 0;
        this.totalPriceFormatted = 0;
        //Salvar los valores reestrablecidos
        this.saveAll();
      } else {
        /*this.file = new File(
          [
            "Cliente:\n" + this.customerName,
            "\nCodigo:\n" + this.customerCode,
            "\nProductos:" + this.productsSelected,
            "\nPrecio Total:\n" + parseFloat(this.totalPrice).toFixed(2),
            "\nColecciones:\n" + parseFloat(this.collections).toFixed(2),
          ],
          "Pedido de " + this.customerCode + ".csv",
          { type: "data:text/csv;charset=utf-8,%EF%BB%BF" }
        );
        FileSaver.saveAs(this.file);*/
      }
    },
  },
  computed: {
    filtered() {
      return this.productsData.filter((productSelected) =>
        productSelected.name.match(new RegExp(this.search, "gi"))
      );
    },
    paginated() {
      return this.filtered.slice(this.offset, this.limit + this.offset);
    },
    hasNextPage() {
      const nextOffset = this.offset + 5;
      return Boolean(
        this.filtered.slice(nextOffset, this.limit + nextOffset).length
      );
    },
    hasPrevPage() {
      const prevOffset = this.offset - 5;
      return Boolean(
        this.filtered.slice(prevOffset, this.limit + prevOffset).length
      );
    },
    unitPriceFormat() {
      return parseFloat(this.productSelected.price).toFixed(2);
    },
    subtotalFormat() {
      return (this.productSelected.price * this.quantity).toFixed(2);
    },
  },
  watch: {},
};
</script>

<style>
.bg-button {
  background: #94c11e;
}
.bg-button-hover:hover {
  background: #94c11e;
}
.color-button-green {
  color: #94c11e;
}
.color-button-blue {
  color: #244a8b;
}
.color-button:hover {
  color: #94c11e;
}
.border-button:hover {
  border: 1px solid #94c11e;
}
.border-button-green {
  border: 1px solid #94c11e;
}
.vs__dropdown-toggle {
  border: 0px solid rgba(0, 0, 0, 0);
}
tr:nth-child(even) {
  background-color: #edf2f7;
}
</style>
