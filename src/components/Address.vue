<template>
  <div slot-scope="{ address }">
    <div class="accordion">
      <div class="card">
        <div class="card-header">
          <div class="row">
            <div class="col-3">
              <label
                data-toggle="collapse"
                :data-target="'#collapse' + index"
                aria-expanded="true"
                aria-controls="collapseOne"
                class="text-primary label-cep mr-5"
              >
                {{ address.cep }}
              </label>
            </div>
            <div class="col-7">
              <label class="text-secondary">{{
                address.nickname || "Defina um apelido para o local..."
              }}</label>
            </div>
            <div class="col-2">
              <div class="float-right">
                <i
                  class="fa fa-pencil text-warning  delete-address ml-3"
                  v-on:click="selectAddress"
                />
                <i
                  class="fa fa-times text-danger  delete-address ml-3"
                  v-on:click="deletedAddress"
                />
              </div>
            </div>
          </div>
        </div>

        <div :id="'collapse' + index" class="collapse">
          <div class="card-body">
            <ul class="list-group">
              <li class="list-group-item">{{ address.logradouro }}</li>
              <li class="list-group-item">
                {{ address.complemento || "Sem complemento" }}
              </li>
              <li class="list-group-item">{{ address.bairro }}</li>
              <li class="list-group-item">{{ address.localidade }}</li>
              <li class="list-group-item">{{ address.uf }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Address",
  props: {
    address: Object,
    index: Number
  },
  methods: {
    selectAddress() {
      localStorage.setItem("selectedAddress", JSON.stringify(this.address));
      this.$root.$emit("selectedAddress", this.index);
    },
    deletedAddress() {
      if (confirm("Tem certeza?")) {
        this.$root.$emit("deletedAddress", this.index);
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

.delete-address,
.edit-address,
.label-cep {
  cursor: pointer;
}
</style>
