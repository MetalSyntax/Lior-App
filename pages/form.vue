<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto overflow-hidden">
    <h1 class="block w-full text-brown-800 text-center text-lg bold py-2 uppercase">
      Bienvenido!,
      <span class="block w-full font-bold">{{ customerName }}</span>
    </h1>
    <form class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto h-full">
      <!--Producto-->
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3 z-20">
        <label
          class="block uppercase tracking-wide text-brown-800 text-xs font-bold mb-2 z-20"
          for="product"
        >Seleccione un producto</label>
        <v-select
          class="block appearance-none w-full bg-brown-200 border  font-semibold border-brown-800 text-brown-800 text-xs rounded leading-tight focus:outline-none focus:bg-white focus:border-brown-400 mb-4 p-2 z-20"
          v-model="productSelected"
          :options="paginated"
          @search="query => (search = query)"
          :filterable="false"
          :value="paginated.name"
          :clearable="false"
          :selectable="options => options.value != false"
          label="name"
          placeholder="INGRESE NOMBRE"
          id="product"
          name="product"
        >
          <li slot="list-footer" class="flex z-30">
            <button
              class="flex-grow hover:pointer text-brown-800 py-1 font-semibold border-t border-brown-800"
              @click.prevent="offset -= 5"
              :disabled="!hasPrevPage"
            >Previo</button>
            <button
              class="flex-grow hover:pointer text-brown-800 py-1 font-semibold border-l border-t border-brown-800"
              @click.prevent="offset += 5"
              :disabled="!hasNextPage"
            >Siguiente</button>
          </li>
        </v-select>
      </section>
      <!--Cantidad-->
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3 z-10">
        <label
          class="block uppercase tracking-wide text-brown-800 text-xs font-bold mb-2 z-10"
          for="quantity"
        >Ingrese cantidad</label>
        <input
          class="appearance-none block w-full bg-brown-200 text-brown-800 text-sm border rounded py-3 px-4 leading-tight focus:outline-none font-semibold  border-brown-400 focus:bg-brown-200 focus:text-brown-800 mb-4 z-10"
          id="quantity"
          type="number"
          placeholder="0"
          maxlength="4"
          min="1"
          max="9999"
          value="0"
          v-model="quantity"
          name="quantity"
        />
      </section>
      <!--Resumen Temporal-->
      <section class="w-full lg:w-full px-3 mt-2 mb-16 lg:mb-8 lg:my-3">
        <label
          class="block uppercase tracking-wide text-brown-800 text-xs font-bold mb-2"
          for="grid-code"
        >Resumen parcial</label>
        <div
          class="appearance-none flex w-full bg-brown-700 text-brown-300 border rounded py-3 px-4 leading-tight border-brown-300"
        >
          <div class="flex flex-wrap w-1/2">
            <p class="text-sm uppercase">Precio Unitario:</p>
            <span
              v-if="productSelected.price > 0"
              class="text-xl text-white w-full font-bold"
            >{{ unitPriceFormat }}$</span>
            <span v-else class="text-xl text-white w-full font-bold">0$</span>
          </div>
          <div class="flex flex-wrap w-1/2">
            <p class="text-sm uppercase">Subtotal:</p>
            <span
              v-if="productSelected.price > 0"
              class="text-xl text-white w-full font-bold"
            >{{ subtotalFormat }}$</span>
            <span v-else class="text-xl text-white w-full font-bold">0$</span>
          </div>
        </div>
      </section>
    </form>
    <!--Resumen-->
    <section v-if="allProducts.length > 0" class="flex flex-wrap w-full lg:w-4/5 px-2 justify-center pb-20 my-0 mx-auto">
      <div
        class="appearance-none flex flex-wrap w-full bg-white text-brown-800 border border-brown-200 rounded py-2 px-2 leading-tight shadow-md"
      >
        <span
          class="block w-full uppercase tracking-wide text-center color-blue-custom text-lg font-bold mb-2"
        >Resumen del Pedido</span>
        <div class="flex flex-wrap w-1/2 text-center">
          <span class="uppercase text-brown-400  text-xs font-bold w-full">Total:</span>
          <span class="text-brown-700  font-bold text-2xl w-full">{{ totalPriceFormatted }}$</span>
        </div>
        <div class="flex flex-wrap w-1/2 text-center">
          <span class="uppercase text-brown-400  text-xs font-bold w-full">Colecciones:</span>
          <span class="text-brown-700  font-bold text-2xl w-full">
            {{
            collectionsFormatted
            }}
          </span>
        </div>
      </div>
      <div
        class="appearance-none flex flex-wrap w-full bg-white text-brown-700 py-2 px-2 leading-tight mt-4 mb-4"
      >
      <span
          class="block w-full uppercase tracking-wide text-center text-sm font-bold mb-2"
        >Calculo de colecciones</span>
        <div class="flex flex-wrap w-full text-center justify-center">
          <span class="uppercase text-brown-200 text-sm font-semibold pr-1 pb-1">Sobrante: </span>
          <span class="text-brown-700 text-sm font-bold">{{surplusFormatted}}$</span>
        </div>
        <div class="flex flex-wrap w-full text-center justify-center">
          <span class="uppercase text-brown-200 text-sm font-semibold pr-1 pb-1">Proxima:</span>
          <span class="text-brown-700 text-sm font-bold">
            {{nextFormatted}}$
          </span>
      </div>
      </div>
      <span
        class="block uppercase tracking-wide text-brown-800 text-lg font-bold my-3"
      >Listado de productos</span>
      <table name="listado" class="w-full table-auto">
        <thead class="border border-brown-800 bg-brown-800">
          <tr>
            <th class="px-2 py-1 text-xs md:text-sm uppercase text-left text-white">Producto</th>
            <th class="px-2 py-1 text-xs md:text-sm uppercase"></th>
          </tr>
        </thead>
        <tbody class="border border-brown-400">
          <tr v-for="(allProduct, index) in allProducts" v-bind:key="index" class="my-2">
            <td class="flex px-1 py-1 text-left text-xs md:text-sm">
              <div class="flex justify-center self-center w-2/12 lg:w-1/12">
              <img class="rounded-full border h-10 lg:h-12"
              :src="require(`@/assets/img/icons/${allProduct.img}`)"
              :alt="allProduct.name">
              </div>
              <div class="w-10/12 lg:w-11/12 ml-1">
              <span class="w-full block font-bold text-brown-800">{{ allProduct.name }}</span>
              <span class="w-full uppercase text-brown-800">Unidades: {{ allProduct.quantity }} -</span>
              <span class="w-full uppercase text-brown-800">Subtotal: {{ allProduct.subtotalformatted }}$</span>
              </div>
            </td>
            <td class="text-center">
              <button
                class="rounded my-0 mx-auto py-1 px-3 cursor-pointer text-lg text-red-500 hover:text-red-700 font-bold focus:border-0 hover:border-0 outline-none hover:outline-none focus:outline-none"
                @click="removeEach(index)"
                title="eliminar"
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
        class="bg-white text-brown-700 border-brown-700 hover:text-brown-400 hover:border-brown-400 border-2 mx-2 my-2 py-2 px-4 rounded cursor-pointer uppercase text-sm focus:outline-none hove:outline-none"
        @click.prevent="saveArchive"
        title="Guardar"
      >Guardar</button>
      <button
        class="bg-brown-700 text-brown-300 border-brown-700 hover:border-brown-200  border-2 mx-2 my-2 py-2 px-8 rounded cursor-pointer uppercase text-sm focus:outline-none hove:outline-none"
        @click.prevent="addQuantity"
        title="Agregar"
      >Agregar</button>
    </section>
  </main>
</template>

<script>
import productsDataJson from "../data/products.json";
import { saveAs } from "file-saver";
const FileSaver = require("file-saver");

export default {
  head() {
    return {
      title: "Lior Cosmetics | Pedidos | Formulario",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Formulario para generar pedidos en Lior Cosmetics",
        },
      ],
    };
  },
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
      next: 0, //
      nextFormatted: 0, //
      surplus: 0, //
      surplusFormatted: 0, //
      quantity: null, // Cantidad
      file: null, //Data del Archivo
      search: "",
      offset: 0,
      limit: 5, //Limite de clientes visibles
    };
  },
  mounted() {
    this.productsData.forEach((data) => {
      data.value = true;
    })
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
    // Muestra el precio total
    if (localStorage.getItem("next")) {
      this.next = JSON.parse(localStorage.getItem("next"));
    }
    if (localStorage.getItem("nextFormatted")) {
      this.nextFormatted = JSON.parse(
        localStorage.getItem("nextFormatted")
      );
    }
    // Muestra el precio total
    if (localStorage.getItem("surplus")) {
      this.surplus = JSON.parse(localStorage.getItem("surplus"));
    }
    if (localStorage.getItem("surplusFormatted")) {
      this.surplusFormatted = JSON.parse(
        localStorage.getItem("surplusFormatted")
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

      this.next = (1-(this.collections-parseInt(this.collections)))*50;
      this.nextFormatted = ((1-(this.collections-parseInt(this.collections)))*50).toFixed(2);

      this.surplus = (this.collections-parseInt(this.collections))*50;
      this.surplusFormatted = ((this.collections-parseInt(this.collections))*50).toFixed(2);

      this.allProducts.push({
        code: this.productSelected.code,
        name: this.productSelected.name,
        img: this.productSelected.img,
        value: (this.productSelected.value = false),
        quantity: parseInt(this.quantity),
        unitedPrice: parseFloat(this.productSelected.price),
        subtotal: parseFloat(this.productSelected.price * this.quantity),
        subtotalformatted: parseFloat(
          this.productSelected.price * this.quantity
        ).toFixed(2),
      });
      this.productsSelected.push([
        "\n",
        this.productSelected.code,
        this.productSelected.name,
        parseInt(this.quantity),
        parseFloat(this.productSelected.price).toFixed(2),
        parseFloat(this.productSelected.price * this.quantity).toFixed(2),
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
      const totalPriceFormatted = JSON.stringify(this.totalPriceFormatted);
      localStorage.setItem("totalPriceFormatted", totalPriceFormatted);

      const collections = JSON.stringify(this.collections);
      localStorage.setItem("collections", collections);
      const collectionsFormatted = JSON.stringify(this.collectionsFormatted);
      localStorage.setItem("collectionsFormatted", collectionsFormatted);

      const next = JSON.stringify(this.next);
      localStorage.setItem("next", next);
      const nextFormatted = JSON.stringify(this.nextFormatted);
      localStorage.setItem("nextFormatted", nextFormatted);

      const surplus = JSON.stringify(this.surplus);
      localStorage.setItem("surplus", surplus);
      const surplusFormatted = JSON.stringify(this.surplusFormatted);
      localStorage.setItem("surplusFormatted", surplusFormatted);
    },
    removeEach(x) {
      this.productsData.forEach((data) => {
        if (data.code == this.allProducts[x].code) {
          data.value = true;
        }
      });

      this.totalPrice -= parseFloat(this.allProducts[x].subtotal);
      this.totalPriceFormatted -= parseFloat(
        this.allProducts[x].subtotal
      ).toFixed(2);

      this.collections = this.totalPrice / 50;
      this.collectionsFormatted = parseFloat(this.totalPrice / 50).toFixed(2);

      this.next = (1-(this.collections-parseInt(this.collections)))*50;
      this.nextFormatted = ((1-(this.collections-parseInt(this.collections)))*50).toFixed(2);

      this.surplus = (this.collections-parseInt(this.collections))*50;
      this.surplusFormatted = ((this.collections-parseInt(this.collections))*50).toFixed(2);

      this.productsSelected.splice(x, 1);
      this.allProducts.splice(x, 1);
      this.saveAll();
    },
    saveArchive(x) {
      if (confirm("¿Deseas guardar y borrar?")) {
        this.file = new File(
          [
            ",Cliente\n," + this.customerName,
            "\n,Código\n," + this.customerCode,
            "\n,Código,Productos,Cantidad,Precio Unitario,Subtotal" +
              this.productsSelected,
            "\n,Precio Total\n," + parseFloat(this.totalPrice).toFixed(2),
            "\n,Colecciones\n," + parseFloat(this.collections).toFixed(2),
            "\n,Proxima Colección\n," + (this.next).toFixed(2),
            "\n,Sobrante\n," + (this.surplus).toFixed(2),
          ],
          "Pedido de " +
            this.customerCode +
            " del " +
            new Date().getDate() +
            "-" +
            (new Date().getMonth()+1) +
            "-" +
            new Date().getFullYear() +
            ".txt",
          {
            type: "text/html;charset=utf-8",
            //type: "data:text/csv;charset=utf-8,%EF%BB%BF"
          }
        );
        FileSaver.saveAs(this.file);
        //Borrar Visual
        this.allProducts.splice(x);
        //Restablecer Valores
        this.totalPrice = 0;
        this.collections = 0;
        this.next = 50;
        this.surplus = 0;
        this.quantity = null;
        this.productsSelected = [];
        this.collectionsFormatted = 0;
        this.totalPriceFormatted = 0;
        this.nextFormatted = 50;
        this.surplusFormatted = 0;
        this.productsData.forEach((data) => {
           data.value = true;
        });
        //Salvar los valores reestrablecidos
        this.saveAll();
      } else {
        console.log("Tu pedido no se ha generado");
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
input::placeholder {
  @apply text-brown-800;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
   @apply appearance-none;
}
.vs__dropdown-toggle {
  @apply border-0;
}
.vs__actions {
 @apply hidden;
}
.vs__dropdown-menu {
   @apply z-30 p-0 border border-brown-800;
}
tr:nth-child(even) img {
   @apply bg-brown-400;
}
tr:nth-child(odd) img {
  @apply bg-brown-200;
}
</style>
