<template>
  <section class="flex flex-wrap w-full px-3 my-2 lg:my-3">
    <label
      class="block uppercase tracking-wide text-gray-900 text-xs font-bold mb-4"
      for="grid-code"
      >Ingrese su nombre</label
    >
    <v-select
      class="block appearance-none w-full bg-gray-200 border border-gray-300 text-gray-900 text-xs rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 mb-6 p-2"
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
      v-if="customerName.length == 0"
      class="block bg-button text-white hover:bg-white color-button border-button border-transparent border py-2 px-4 rounded cursor-pointer uppercase text-sm my-0 mx-auto"
      @click.prevent="addCustomer"
    >
      Guardar cliente
    </button>
    <nuxt-link
      v-if="customerName.length > 0"
      class="block bg-button text-white hover:bg-white color-button border-button border-transparent border py-2 px-4 rounded cursor-pointer uppercase text-sm my-0 mx-auto"
      to="/form"
      >Continuar</nuxt-link
    >
  </section>
</template>

<script>
import customersDataJson from "../data/customers.test.json";

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
  mounted() {
  },
  methods: {
    addCustomer() {
      localStorage.clear();
      this.customerCode = this.customer.code;
      this.customerName = this.customer.name;
      this.saveCustomer();
    },
    saveCustomer() {
      const customerName = JSON.stringify(this.customerName);
      localStorage.setItem("customerName", customerName);
      const customerCode = JSON.stringify(this.customerCode);
      localStorage.setItem("customerCode", customerCode);
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
  }
};
</script>

<style>
.bg-button {
  background: var(--color-primary);
}
.color-button:hover {
  color: var(--color-primary);
}
.border-button:hover {
  border: 1px solid var(--color-primary);
}
.vs__dropdown-toggle {
  border: 0px solid black;
}
</style>
