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
                <v-img :src="product.image" height="200" contain />
              </v-card-text>
              <v-card-title>
                {{ product.name }} ({{ product.quantity }})
              </v-card-title>
              <v-card-subtitle>{{
                product.price.toLocaleString("pt-br", {
                  style: "currency",
                  currency: "BRL",
                })
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
    />
  </v-item-group>
</template>

<script>
import ProductDialog from "./ProductDialog.vue";

import productList from "@/placeholders/productList.json";

export default {
  name: "ProductList",
  components: {
    ProductDialog,
  },
  data: () => ({
    productList: productList.products,
    selectedProduct: {},
    isDialogVisible: false,
  }),
  methods: {
    onClickProduct(product) {
      this.selectedProduct = product;
      this.isDialogVisible = true;
    },
  },
};
</script>