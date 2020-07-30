<template>
  <main class="container flex items-center flex-wrap w-full my-0 mx-auto">
    <form v-on:submit.prevent="addQuantity" class="flex flex-wrap justify-center py-4 w-full max-w-8xl my-0 mx-auto">
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
        <!--<span v-if="customers.code" class="block w-full text-gray-900 text-center">Hola!, {{ customers.name }}</span>-->
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
            class="priceProduct text-lg text-gray-700"
          >{{ products.price }}$</span>
          <span v-else class="priceProduct text-gray-700">No hay valor del producto</span>
          <br />
          <p>Precio Total:</p>
          <span
            v-if="products.price > 0"
            class="totalProducts text-lg text-gray-700"
          >{{products.price * quantity}}$</span>
          <span v-else class="totalProducts text-gray-700">No hay un producto seleccionado</span>
        </div>
      </section>
      <section class="flex w-full lg:w-full px-3 my-2 lg:my-3 justify-center">
        <input
          class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded"
          type="submit"
          value="Agregar producto">
      </section>
    </form>
    <section class="flex flex-wrap w-full px-2 my-2 lg:my-3 justify-center">
      <span
        class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-2"
      >Listado de productos seleccionados</span>
      <!--<div>
        <ul v-for="productByCustomer in productsByCustomer" :key="productByCustomer">
          <li>{{productsByCustomer.code}}</li>
          <li>{{productsByCustomer.name}}</li>
          <li>{{productsByCustomer.price}}</li>
          <li>{{productsByCustomer.quantity}}</li>
        </ul>
      </div>-->
      <!--<section
      v-for="productByCustomer in productsByCustomer"
      v-bind:key="productByCustomer"
      class="w-full flex flex-wrap border-gray-500 border-solid border-b-2 mx-2">
        {{ productByCustomer }}
        <span class="w-full text-xs">EX-0002</span>
        <span class="w-full text-ms">Post - Coco 240 ML</span>
        <span class="w-full text-ms">Precio: <span class="text-lg">6$</span></span>
      </section>-->
    </section>
  </main>
</template>

<script>
export default {
  /*data: {
    quantityByCustomer: [],
    code: "",
    name: "",
    price: "",
    quantity: "",
    productsByCustomer: '',
    products: null,
  },*/
  data() {
    return {
      quantity: '',
      customer: '',
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
      /*productsByCustomer: [
        {
          code: "",
          name: "",
          price: "",
          quantity: "",
        },
      ],*/
    };
  },
  mounted() {
    if (localStorage.quantity) {
      this.quantity = localStorage.quantity;
    }
    if (localStorage.customer) {
      this.customer = localStorage.customer;
    }
    /*if (localStorage.getItem('productsByCustomer')) {
      try {
        this.productsByCustomer = JSON.parse(localStorage.getItem('productsByCustomer'));
      } catch(e) {
        localStorage.removeItem('productsByCustomer');
      }
    }*/
  },
  methods: {
    addQuantity() {
      if (!this.quantity) {
        return;
      }
      if (!this.customer) {
        return;
      }
      localStorage.quantity = this.quantity;
      localStorage.customer = this.customer;
      this.quantity = '';
      this.customer = '';
      /*this.saveProducts()*/
    },
    /*addQuantity: function() {
      localStorage.quantity = newQuantity;
    },*/
    /*addQuantity: function () {
      this.quantityByCustomer.push({
        quantity: this.quantity
      });
      console.log(quantityByCustomer)
      this.quantity = '';
    }*/
    /*addProducts: function() {
          this.productsByCustomer.push({
            code: this.products.code,
            name: this.products.name,
            price: this.products.price,
            quantity: this.products.quantity
        });
      this.products.code = '';
      this.products.name = '';
      this.products.price = '';
      this.products.quantity = '';
    }*/
    /*addProduct() {
      if (!this.products.name) {
        return;
      }
      if (!this.products.code) {
        return;
      }
       if (!this.products.price) {
        return;
      }
      this.productsByCustomer.push(this.products.name);
      this.productsByCustomer.push(this.products.code);
      this.productsByCustomer.push(this.products.price);
      this.products.name = '',
      this.products.code = '',
      this.products.price = '';
      this.saveProducts();
    },*/
    /*removeCat(x) {
      this.cats.splice(x, 1);
      this.saveCats();
    },*/
    /*saveProducts() {
      const parsed = JSON.stringify(this.customer);
      localStorage.setItem("customer", parsed);
    },*/
  },
  computed: {
    /*addQuantity: function() {
      this.quantityByCustomer.push(this.quantity);
      console.log(quantityByCustomer)
      this.quantity = '';
    }*/
  },
  /*watch:{
    quantity(newQuantity) {
      localStorage.quantity = newQuantity;
    }
  }*/
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
