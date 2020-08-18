<template>
  <form class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto h-full">
      <!--Producto-->
      <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
        <label
          class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
          for="grid-product"
        >Seleccione un producto</label>
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
      <!--Resumen Temporal-->
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
          <span v-else class="text-md text-gray-700">No hay valor del producto</span>
          <br />
          <p class="text-sm uppercase">Precio Subtotal:</p>
          <span
            v-if="productSelected.price > 0"
            class="text-xl text-gray-700"
          >{{ productSelected.price * quantity }}$</span>
          <span v-else class="text-md text-gray-700">No hay un producto seleccionado</span>
        </div>
      </section>
      <!--Envio-->
      <section class="flex w-full lg:w-full px-3 my-2 lg:my-3 justify-center">
        <button
          class="bg-button text-white hover:bg-white color-button border-button border-transparent border py-2 px-4 rounded cursor-pointer uppercase text-sm"
          @click.prevent="addQuantity"
        >Agregar producto</button>
      </section>
    </form>
</template>

<script>
import productsDataJson from "../data/products.json";

export default {
  name: "Products",
  data() {
    return {
      allProducts: [], // Arreglo de todos los productos
      productSelected: [], // Producto seleccionado por el cliente
      productsSelected: [], // Productos guardados en arreglo
      productsData: Object.values(productsDataJson), //Data de productos
      totalPrice: 0, // Precio Total
      totalPriceFormatted: 0, //
      collections: 0, //Precio Total
      collectionsFormatted: 0, //
      quantity: null, // Cantidad
    };
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
        this.productSelected.price,
        this.productSelected.price * this.quantity,
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
  },
};
</script>

<style>
.vs__dropdown-toggle {
  border: 0px solid black;
}
</style>
