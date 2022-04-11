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

    <v-dialog v-model="isDialogVisible" width="500">
      <v-card>
        <v-card-text class="py-4 px-6">
          <v-img :src="selectedProduct.image" height="200" contain />
        </v-card-text>

        <v-card-title class="text-h5">
          {{ selectedProduct.name }}
        </v-card-title>
        <v-card-subtitle>{{
          selectedProduct.price
            ? selectedProduct.price.toLocaleString("pt-br", {
                style: "currency",
                currency: "BRL",
              })
            : ""
        }}</v-card-subtitle>

        <v-card-text>
          <v-row>
            <v-col cols="12" md="4" class="py-0">
              Estoque: {{ selectedProduct.quantity }}
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
          <v-btn color="primary" text @click="isDialogVisible = false">
            Cancelar
          </v-btn>
          <v-btn color="primary" @click="isDialogVisible = false">
            Comprar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-item-group>
</template>

<script>
import productList from "@/placeholders/productList.json";

export default {
  name: "ProductList",
  data: () => ({
    productList: productList.products,
    selectedProduct: {},
    quantityOptions: [],
    selectedQuantity: 1,
    isDialogVisible: false,
  }),
  methods: {
    onClickProduct(product) {
      this.selectedProduct = product;
      const maximumQuantity =
        this.selectedProduct.quantity > 10 ? 10 : this.selectedProduct.quantity;
      this.quantityOptions = Array.from(
        Array(maximumQuantity),
        (element, index) => index + 1
      );

      this.isDialogVisible = true;
    },
  },
};
</script>