<template>
  <div v-if="addresses.length > 0" class="mt-3">
    <div v-for="(address, index) in addresses" v-bind:key="index">
      <Address :address="address" :index="index" />
    </div>
  </div>
  <div v-else>
    <div class="alert alert-danger mt-3">
      Nenhum endereço cadastro ainda.
    </div>
  </div>
</template>

<script>
import Address from "./Address.vue";
export default {
  name: "List",
  props: {
    addresses: Array
  },
  components: {
    Address
  }
};
</script>
<style></style>
