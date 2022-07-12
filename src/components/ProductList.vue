<template>
  <v-item-group>
    <v-container>
      <v-row>
        <v-col
          v-for="product in productList"
          :key="product.id"
          cols="12"
          md="4"
        >
          <v-item>
            <v-card class="ma-0" @click="onClickProduct(product)">
              <v-card-text>
                <v-img :src="product.foto" height="200" contain />
              </v-card-text>
              <v-card-title>
                {{ product.nome }} ({{ product.quantidade }})
              </v-card-title>
              <v-card-subtitle>{{
                product.preco
                  ? product.preco.toLocaleString("pt-br", {
                      style: "currency",
                      currency: "BRL",
                    })
                  : ""
              }}</v-card-subtitle>
            </v-card>
          </v-item>
        </v-col>
      </v-row>
    </v-container>

    <ProductDialog
      :isDialogVisible="isDialogVisible"
      :selectedProduct="selectedProduct"
      @close-dialog="isDialogVisible = false"
      @add-to-cart="(product) => $emit('add-to-cart', product)"
    />
  </v-item-group>
</template>

<script>
import ProductDialog from "./Dialogs/ProductDialog.vue";

export default {
  name: "ProductList",
  components: {
    ProductDialog,
  },
  props: {
    productList: Array,
  },
  data: () => ({
    selectedProduct: {},
    isDialogVisible: false,
  }),
  methods: {
    onClickProduct(product) {
      this.selectedProduct = product;
      this.isDialogVisible = true;
    },
  },
  mounted() {
    this.$emit("load-products");
  },
};
</script>