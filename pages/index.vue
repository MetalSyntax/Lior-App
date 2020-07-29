<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto">
    <form class="flex flex-wrap justify-center w-full max-w-5xl my-0 mx-auto">
      <div class="flex flex-wrap w-full mx-3 my-6">
        <section class="w-full lg:w-full px-3 my-2 lg:my-3">
          <label
            class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
            for="grid-code"
          >Ingrese su codigo de cliente</label>
          <input
            class="appearance-none block w-full bg-gray-200 text-gray-900 border rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
            id="grid-code"
            type="text"
            maxlength="7"
            placeholder="Codigo del cliente"
            v-model="customers.code"
          />
          <!--<span class="block w-full text-gray-900 text-center">Hola!, {{ customers.code }}</span>-->
        </section>
        <section class="w-full lg:w-1/2 px-3 my-2 lg:my-3">
          <label
            class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
            for="grid-product"
          >Seleccione un producto</label>
          <!--<select
            class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 py-3 px-4 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
            id="grid-product"
            name="product"
          >
            <option selected="selected" disabled="disabled">Nombre del producto</option>
            <option>Pre - Coco 240ML</option>
            <option>Post - Coco 240 ML</option>
          </select>-->
          <v-select
            class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
            v-model="products.price"
            :options="products"
            :reduce="products => products.price"
            :value="products.name"
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
            v-if="products.price > 0"
            class="priceProduct text-xl text-gray-700">
              {{ products.price }}$
            </span>
            <span
            v-else
            class="priceProduct text-gray-700">
             No hay valor del producto
            </span>
            <br />
            <p>Precio Total:</p>
            <span
              v-if="products.price > 0"
              class="totalProducts text-xl text-gray-700">
              {{products.price * quantity}}$
            </span>
            <span
              v-else
              class="totalProducts text-gray-700">
              No hay un producto seleccionado
            </span>
          </div>
        </section>
      </div>
    </form>
  </main>
</template>

<script>
export default {
  data() {
    return {
      quantity: "",
      products: [
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
          name: "Pre - Coco 240ML",
          price: 5,
        },
      ],
      customers: [
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
