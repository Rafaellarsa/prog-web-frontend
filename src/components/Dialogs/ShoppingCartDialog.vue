<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card>
      <v-card-title class="text-h5"> Carrinho </v-card-title>

      <v-card-text>
        <v-row v-if="!products.length">
          <v-col cols="12" md="12" class="py-0">Seu carrinho está vazio.</v-col>
          <v-spacer />
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
  name: "ShoppingCartDialog",
  props: {
    isDialogVisible: Boolean,
    products: Array,
  },
  data: () => ({
    isVisible: false,
  }),
  watch: {
    isDialogVisible(newValue) {
      this.isVisible = newValue;
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