<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto">
    <form class="flex flex-wrap justify-center w-full max-w-5xl my-0 mx-auto">
      <div class="flex flex-wrap w-full mx-3 my-6">
        <section class="w-full lg:w-full px-3 mb-6 lg:my-3">
          <label
            class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
            for="grid-code"
          >Ingrese su codigo de cliente</label>
          <input
            class="appearance-none block w-full bg-gray-200 text-gray-900 border rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
            id="grid-code"
            type="text"
            placeholder="Codigo del cliente"
          />
        </section>
        <section class="w-full lg:w-1/2 px-3 mb-6 lg:my-3">
          <label
            class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
            for="grid-product"
          >Seleccione un producto</label>
          <select
            class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 py-3 px-4 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
            id="grid-product"
            name="product"
          >
            <option selected="selected" disabled="disabled">Nombre del producto</option>
            <option>Pre - Coco 240ML</option>
            <option>Post - Coco 240 ML</option>
          </select>
        </section>
        <section class="w-full lg:w-1/2 px-3 mb-6 lg:my-3">
          <label
            class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
            for="grid-code"
          >Ingrese la cantidad de los productos</label>
          <input
            class="appearance-none block w-full bg-gray-200 text-gray-900 border rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
            id="grid-quantity"
            type="number"
            placeholder="Cantidad"
          />
        </section>
        <section class="w-full lg:w-full px-3 mb-6 lg:my-3">
          <label
            class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
            for="grid-code"
          >Resumen parcial</label>
          <div
            class="appearance-none block w-full bg-gray-200 text-gray-500 border rounded py-3 px-4 leading-tight"
          >
            <p>Precio por producto:</p>
            <br />
            <p>Cantidad de productos:</p>
            <br />
            <p>Total por productos:</p>
          </div>
        </section>
      </div>
    </form>
  </main>
</template>

<script>
export default {};
</script>

<style>
</style>
