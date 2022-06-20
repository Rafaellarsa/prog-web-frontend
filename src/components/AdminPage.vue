<template>
  <v-container>
    <v-tabs v-model="tab" grow>
      <v-tab>Estoque</v-tab>
      <v-tab>Categorias</v-tab>
      <v-tab>Usuários</v-tab>
      <v-tab>Relatórios</v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab" class="mt-3">
      <v-tab-item>
        <v-btn text @click="isNewProductDialogVisible = true">
          <v-icon aria-label="Mais" color="primary">mdi-plus</v-icon>
          Novo produto
        </v-btn>
        <v-list two-line>
          <v-list-item v-for="product in productList" :key="product.id">
            <v-list-item-avatar>
              <img :src="product.image" :alt="product.name" />
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>
                {{ product.name }} ({{ product.quantity }})
              </v-list-item-title>

              <v-list-item-subtitle>
                {{
                  product.price.toLocaleString("pt-br", {
                    style: "currency",
                    currency: "BRL",
                  })
                }}
                - {{ product.categoryName }}
              </v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-action class="d-flex flex-row align-center">
              <v-btn icon>
                <v-icon color="primary" aria-label="Editar">mdi-pencil</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon color="primary" aria-label="Remover">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-tab-item>
      <v-tab-item>
        <v-btn text @click="isNewCategoryDialogVisible = true">
          <v-icon aria-label="Mais" color="primary">mdi-plus</v-icon>
          Nova categoria
        </v-btn>
        <v-list>
          <v-list-item v-for="category in categories" :key="category.id">
            <v-list-item-content>
              <v-list-item-title>
                {{ category.name }}
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action class="d-flex flex-row align-center">
              <v-btn icon>
                <v-icon color="primary" aria-label="Editar">mdi-pencil</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon color="primary" aria-label="Remover">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-tab-item>
      <v-tab-item>
        <v-btn text>
          <v-icon aria-label="Mais" color="primary">mdi-plus</v-icon>
          Novo usuário
        </v-btn>
      </v-tab-item>
      <v-tab-item> Visualizar relatórios gerenciais </v-tab-item>
    </v-tabs-items>

    <NewProductDialog
      :isDialogVisible="isNewProductDialogVisible"
      @close-dialog="isNewProductDialogVisible = false"
    />
    <NewCategoryDialog
      :isDialogVisible="isNewCategoryDialogVisible"
      @close-dialog="isNewCategoryDialogVisible = false"
    />
  </v-container>
</template>

<script>
import axios from "axios";

import NewProductDialog from "./NewProductDialog.vue";
import NewCategoryDialog from "./NewCategoryDialog.vue";

import productList from "@/placeholders/productList.json";
import categories from "@/placeholders/categories.json";

export default {
  name: "AdminPage",
  components: {
    NewProductDialog,
    NewCategoryDialog,
  },
  data: () => ({
    tab: null,
    isNewProductDialogVisible: false,
    isNewCategoryDialogVisible: false,
    categories: categories.categories,
    productList: productList.products,
  }),
  mounted() {
    axios
      .get("http://localhost:8080/e-commerce/ListarProdutos")
      .then((response) => console.log(response.data));
  },
};
</script>