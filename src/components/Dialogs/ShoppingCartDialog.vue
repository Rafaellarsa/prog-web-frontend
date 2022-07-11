<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card>
      <v-card-title class="text-h5"> Carrinho </v-card-title>

      <v-card-text>
        <v-row>
          <v-col v-if="!products.length" cols="12" md="12" class="py-0">
            Seu carrinho está vazio.
          </v-col>

          <v-list v-else two-line width="100%">
            <v-list-item v-for="product in products" :key="product.id">
              <v-list-item-avatar>
                <img :src="product.foto" :alt="product.nome" />
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title>
                  {{ product.nome }} ({{ product.quantidade }})
                </v-list-item-title>

                <v-list-item-subtitle>
                  {{
                    product.preco
                      ? product.preco.toLocaleString("pt-br", {
                          style: "currency",
                          currency: "BRL",
                        })
                      : null
                  }}
                  - Categoria: {{ product.id_categoria }}
                </v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-action class="d-flex flex-row align-center">
                <v-select
                  class="quantity-select"
                  label="Quantidade"
                  :items="getQuantityOptions(product.quantidade)"
                  v-model="product.quantity"
                />
                <v-btn icon>
                  <v-icon
                    color="primary"
                    aria-label="Remover"
                    @click="removeProduct(product.id)"
                    >mdi-delete</v-icon
                  >
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list>
        </v-row>
      </v-card-text>
      <v-spacer />
      <v-divider />

      <v-card-actions>
        <v-spacer />
        <v-btn color="primary" text @click="closeDialog()"> Cancelar </v-btn>
        <v-btn
          color="primary"
          :disabled="!user.login || !products.length"
          @click="proceed()"
        >
          Prosseguir
        </v-btn>
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
    user: Object,
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
    getQuantityOptions(quantity) {
      const maximumQuantity = quantity > 10 ? 10 : quantity;
      return Array.from(Array(maximumQuantity), (element, index) => index + 1);
    },
    proceed() {
      console.log(this.products);
      this.closeDialog();
    },
    closeDialog() {
      this.isVisible = false;
    },
  },
};
</script>

<style scoped>
.quantity-select {
  width: 4rem;
}
</style>