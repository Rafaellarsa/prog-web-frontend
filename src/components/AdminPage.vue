<template>
  <v-container>
    <v-tabs v-model="tab" grow>
      <v-tab>Estoque</v-tab>
      <v-tab>Categorias</v-tab>
      <v-tab>Vendas</v-tab>
      <v-tab>Relatórios</v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab" class="mt-3">
      <v-tab-item>
        <v-btn text @click="isNewProductDialogVisible = true">
          <v-icon aria-label="Mais" color="primary">mdi-plus</v-icon>
          Novo produto
        </v-btn>
        <v-list v-if="products.length" two-line>
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
              <v-btn icon>
                <v-icon
                  color="primary"
                  aria-label="Editar"
                  @click="editProduct(product)"
                  >mdi-pencil</v-icon
                >
              </v-btn>
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
      </v-tab-item>

      <v-tab-item>
        <v-btn text @click="isNewCategoryDialogVisible = true">
          <v-icon aria-label="Mais" color="primary">mdi-plus</v-icon>
          Nova categoria
        </v-btn>
        <v-list v-if="categories.length">
          <v-list-item v-for="category in categories" :key="category.id">
            <v-list-item-content>
              <v-list-item-title>
                {{ category.descricaoCategoria }}
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action class="d-flex flex-row align-center">
              <v-btn icon>
                <v-icon
                  color="primary"
                  aria-label="Editar"
                  @click="editCategory(category)"
                  >mdi-pencil</v-icon
                >
              </v-btn>
              <v-btn icon>
                <v-icon
                  color="primary"
                  aria-label="Remover"
                  @click="removeCategory(category.id)"
                  >mdi-delete</v-icon
                >
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-tab-item>

      <v-tab-item>
        <v-list v-if="sales.length">
          <v-list-item v-for="sale in sales" :key="sale.id_venda">
            <v-list-item-content>
              <v-list-item-title>
                Id: {{ sale.id_venda }} - Produto: {{ sale.id_produto_venda }} -
                Cliente: {{ sale.id_cliente_venda }} -
                {{
                  new Date(sale.data_hora_venda.slice(0, 29)).toLocaleString()
                }}
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action class="d-flex flex-row align-center">
              <v-btn icon>
                <v-icon
                  color="primary"
                  aria-label="Remover"
                  @click="removeSale(sale.id_venda)"
                  >mdi-delete</v-icon
                >
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-tab-item>

      <v-tab-item>
        <v-list v-if="relatorios.length" two-line>
          <v-list-item v-for="relatorio in relatorios" :key="relatorio.id">
            <v-list-item-content>
              <v-list-item-title>
                {{ relatorio.movimentacao }} do produto
                {{ relatorio.nome_produto }} de id {{ relatorio.id }} em
                {{ relatorio.quantidade }} quantidades na data
                {{ relatorio.data_hora_venda }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-tab-item>
    </v-tabs-items>

    <NewProductDialog
      :isDialogVisible="isNewProductDialogVisible"
      @close-dialog="isNewProductDialogVisible = false"
      @reload-products="loadProducts()"
    />
    <NewCategoryDialog
      :isDialogVisible="isNewCategoryDialogVisible"
      @close-dialog="isNewCategoryDialogVisible = false"
      @reload-categories="loadCategories()"
    />

    <EditProductDialog
      :isDialogVisible="isEditProductDialogVisible"
      :selectedProduct="selectedProduct"
      @close-dialog="isEditProductDialogVisible = false"
      @reload-products="loadProducts()"
    />
    <EditCategoryDialog
      :isDialogVisible="isEditCategoryDialogVisible"
      :selectedCategory="selectedCategory"
      @close-dialog="isEditCategoryDialogVisible = false"
      @reload-categories="loadCategories()"
    />
  </v-container>
</template>

<script>
import axios from "axios";

import NewProductDialog from "./Dialogs/NewProductDialog.vue";
import NewCategoryDialog from "./Dialogs/NewCategoryDialog.vue";
import EditProductDialog from "./Dialogs/EditProductDialog.vue";
import EditCategoryDialog from "./Dialogs/EditCategoryDialog.vue";

export default {
  name: "AdminPage",
  components: {
    NewProductDialog,
    NewCategoryDialog,
    EditProductDialog,
    EditCategoryDialog,
  },
  data: () => ({
    tab: null,
    isNewProductDialogVisible: false,
    isNewCategoryDialogVisible: false,
    isEditProductDialogVisible: false,
    isEditCategoryDialogVisible: false,
    products: [],
    categories: [],
    sales: [],
    relatorios: [],
    selectedProduct: null,
    selectedCategory: null,
  }),
  watch: {
    tab(newValue) {
      if (newValue == 1 && !this.categories.length) {
        this.loadCategories();
      } else if (newValue == 2 && !this.sales.length) {
        this.loadSales();
      }
    },
  },
  mounted() {
    this.loadProducts();
    this.loadRelatorios();
  },
  methods: {
    loadProducts() {
      axios
        .get("http://localhost:8080/e-commerce/ListarProdutos")
        .then((response) => {
          this.products = response.data;
        })
        .catch((error) => console.log(error));
    },
    loadCategories() {
      axios
        .get("http://localhost:8080/e-commerce/ListarCategorias")
        .then((response) => {
          this.categories = response.data;
        })
        .catch((error) => console.log(error));
    },
    loadSales() {
      axios
        .get("http://localhost:8080/e-commerce/ListarVendas")
        .then((response) => {
          this.sales = response.data;
        })
        .catch((error) => console.log(error));
    },
    loadRelatorios() {
      axios
        .get("http://localhost:8080/e-commerce/Relatorios")
        .then((response) => {
          this.relatorios = response.data;
          console.log(this.relatorios);
        })
        .catch((error) => console.log(error));
    },
    removeProduct(id) {
      axios
        .post(`http://localhost:8080/e-commerce/DeletarProduto?id=${id}`)
        .catch((error) => console.log(error))
        .finally(() => {
          this.loadProducts();
        });
    },
    removeCategory(id) {
      axios
        .post(`http://localhost:8080/e-commerce/DeletarCategoria?id=${id}`)
        .catch((error) => console.log(error))
        .finally(() => {
          this.loadCategories();
        });
    },
    removeSale(id) {
      axios
        .post(`http://localhost:8080/e-commerce/ExcluirOperacao?id=${id}`)
        .catch((error) => console.log(error))
        .finally(() => {
          this.loadSales();
        });
    },
    editProduct(product) {
      this.selectedProduct = product;
      this.isEditProductDialogVisible = true;
    },
    editCategory(category) {
      this.selectedCategory = category;
      this.isEditCategoryDialogVisible = true;
    },
  },
};
</script>