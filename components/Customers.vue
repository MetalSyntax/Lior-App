<template>
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
    >Guardar cliente
  </button>
  </section>
</template>

<script>
import customersDataJson from "../data/customers.json";

export default {
  name: "Customers",
  data() {
    return {
      customer: [], //Cliente
      customerName: "", //Cliente Seleccionado
      customerCode: "", //Cliente guardado
      customersData: Object.values(customersDataJson), //Data de clientes
      search: "",
      offset: 0,
      limit: 3, //Limite de clientes visibles
    };
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
};
</script>

<style>
</style>
