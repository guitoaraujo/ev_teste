<template>
  <div id="app">
    <div class="container">
      <nav class="navbar navbar-light bg-light">
        <span class="navbar-brand mb-0 h1">Meus Endereços</span>
      </nav>
      <div class="row">
        <div class="col-8">
          <List :addresses="addresses" />
        </div>
        <div class="col-4">
          <div class="row">
            <div class="new col-12">
              <div v-if="error" class="alert alert-danger mt-3">
                {{ error }}
              </div>
              <input
                type="text"
                class="form-control mt-3"
                placeholder="Digite o cep"
                v-model="cep"
                @change="getAddress"
              />
            </div>
          </div>
          <div class="row">
            <div v-if="!error" class="edit col-12">
              <FormAddress :address="address" :index="index" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import List from "./components/List.vue";
import FormAddress from "./components/FormAddress.vue";

export default {
  name: "app",
  components: {
    List,
    FormAddress
  },
  data: function() {
    return {
      cep: "",
      addresses: [],
      address: null,
      index: null,
      error: null
    };
  },
  mounted() {
    const localAddresses = JSON.parse(localStorage.getItem("addresses")) || [];
    this.addresses = localAddresses;
    this.$root.$on("postAddress", addresses => {
      this.addresses = addresses;
    });
    this.$root.$on("selectedAddress", index => {
      this.index = index;
      this.address = this.addresses[index];
    });
    this.$root.$on("deletedAddress", index => {
      this.addresses.splice(index, 1);
      if (this.addresses.length > 0) {
        localStorage.setItem("addresses", JSON.stringify(this.addresses));
      } else {
        localStorage.removeItem("addresses");
      }
    });
  },
  methods: {
    getAddress() {
      const url = "https://viacep.com.br/ws/" + this.cep + "/json/";

      axios
        .get(url)
        .then(({ data }) => {
          this.address = data;
          if (this.address.cep) {
            localStorage.setItem(
              "selectedAddress",
              JSON.stringify(this.address)
            );
            this.error = null;
          } else {
            this.error = "Erro, verifique o cep digitado.";
          }
        })
        .catch(() => {
          this.error = "Erro, verifique o cep digitado.";
        });
    }
  }
};
</script>

<style>
#app {
  font-family: "Raleway", sans-serif;
}
</style>
