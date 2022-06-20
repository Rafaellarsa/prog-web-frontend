<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card>
      <v-card-text class="py-4 px-6">
        <v-img :src="selectedProduct.foto" height="200" contain />
      </v-card-text>

      <v-card-title class="text-h5">
        {{ selectedProduct.nome }}
      </v-card-title>
      <v-card-subtitle>{{
        selectedProduct.preco
          ? selectedProduct.preco.toLocaleString("pt-br", {
              style: "currency",
              currency: "BRL",
            })
          : ""
      }}</v-card-subtitle>

      <v-card-text>
        <v-row>
          <v-col cols="12" md="4" class="py-0">
            Estoque: {{ selectedProduct.quantidade }}
          </v-col>
          <v-spacer />
          <v-col cols="12" md="4" class="py-0">
            <v-select
              label="Quantidade"
              :items="quantityOptions"
              v-model="selectedQuantity"
            />
          </v-col>
        </v-row>
      </v-card-text>

      <v-divider />

      <v-card-actions>
        <v-spacer />
        <v-btn color="primary" text @click="closeDialog()"> Cancelar </v-btn>
        <v-btn color="primary" @click="closeDialog()"> Comprar </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "ProductDialog",
  props: {
    isDialogVisible: Boolean,
    selectedProduct: Object,
  },
  data: () => ({
    isVisible: false,
    quantityOptions: [],
    selectedQuantity: 1,
  }),
  watch: {
    isDialogVisible(newValue) {
      this.isVisible = newValue;

      if (newValue) {
        const maximumQuantity =
          this.selectedProduct.quantidade > 10
            ? 10
            : this.selectedProduct.quantidade;
        this.quantityOptions = Array.from(
          Array(maximumQuantity),
          (element, index) => index + 1
        );
      }
    },
    isVisible(newValue) {
      if (!newValue) {
        this.$emit("close-dialog");
      }
    },
  },
  methods: {
    closeDialog() {
      this.isVisible = false;
    },
  },
};
</script>