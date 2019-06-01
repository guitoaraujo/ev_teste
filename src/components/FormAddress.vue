<template>
  <div v-if="address" slot-scope="{ address }" class="mt-3">
    <div class="accordion">
      <div class="card">
        <div class="card-header">
          <h2 class="mb-0 text-center">
            <button
              class="btn btn-link"
              type="button"
              data-toggle="collapse"
              :data-target="'#form-collapse' + index"
              aria-expanded="true"
              aria-controls="collapseOne"
            >
              {{ address.cep }}
            </button>
          </h2>
        </div>

        <div :id="'form-collapse' + index" class="collapse show">
          <div class="card-body">
            <div class="form-group">
              <label>Apelido</label>
              <input
                type="text"
                class="form-control"
                name="name"
                v-model="address.nickname"
              />
              <label>Logradouro</label>
              <input
                type="text"
                class="form-control"
                name="logradouro"
                v-model="address.logradouro"
              />
              <label>Complemento</label>
              <input
                type="text"
                class="form-control"
                v-model="address.complemento"
              />
              <label>Bairro</label>
              <input
                type="text"
                class="form-control"
                v-model="address.bairro"
              />
              <label>Localidade</label>
              <input
                type="text"
                class="form-control"
                v-model="address.localidade"
              />
              <label>UF</label>
              <input type="text" class="form-control" v-model="address.uf" />
            </div>
            <button class="btn btn-primary" v-on:click="postAddress">
              Salvar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FormAddress",
  props: {
    address: Object,
    index: Number
  },
  methods: {
    postAddress() {
      if (this.address.cep) {
        const localAddresses =
          JSON.parse(localStorage.getItem("addresses")) || [];
        if (this.index != null) {
          localAddresses[this.index] = this.address;
        } else {
          localAddresses.push(this.address);
        }
        localStorage.setItem("addresses", JSON.stringify(localAddresses));
        this.$root.$emit("postAddress", localAddresses);
      }
    }
  }
};
</script>
<style>
.card {
  overflow: visible !important;
}
.card-body {
  border: 1px solid rgba(0, 0, 0, 0.125);
}
</style>
