<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-btn class="d-flex align-center" text>
        <v-icon aria-label="Logo e-smd" x-large>mdi-store</v-icon>
        <v-toolbar-title class="shrink mt-1 hidden-sm-and-down" contain>
          e-smd
        </v-toolbar-title>
      </v-btn>

      <v-spacer />
      <span class="mr-2">
        <b
          >Bem-vindo<span v-if="user.login">, {{ user.login }}</span
          >! :)</b
        >
        <br />
        <span
          v-if="!user.login"
          id="login-link"
          @click="isLoginDialogVisible = true"
        >
          Entre ou cadastre-se
        </span>
        <span v-else id="login-link" @click="logout()"> Sair </span>
      </span>
      <v-btn
        v-if="!user.administrador"
        text
        @click="isShoppingCartDialogVisible = true"
      >
        <v-icon aria-label="Carrinho de compras">mdi-cart</v-icon>
        {{ productsTotal }}
      </v-btn>
      <v-btn v-if="user.login" text @click="isUserDialogVisible = true">
        <v-icon aria-label="Engrenagem">mdi-cog</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <AdminPage v-if="user.administrador" />
      <ProductList
        :productList="productList"
        @load-products="loadProducts()"
        @add-to-cart="(product) => addToCart(product)"
        v-else
      />
    </v-main>

    <LoginDialog
      :isDialogVisible="isLoginDialogVisible"
      @close-dialog="isLoginDialogVisible = false"
      @login="onLogin"
    />
    <UserDialog
      :isDialogVisible="isUserDialogVisible"
      :user="user"
      @close-dialog="isUserDialogVisible = false"
      @login="onLogin"
    />
    <ShoppingCartDialog
      :isDialogVisible="isShoppingCartDialogVisible"
      :products="shoppingCartList"
      :user="user"
      @remove-from-cart="removeFromCart"
      @load-products="loadProducts()"
      @close-dialog="isShoppingCartDialogVisible = false"
    />
  </v-app>
</template>

<script>
import axios from "axios";

import ProductList from "./components/ProductList.vue";
import AdminPage from "./components/AdminPage.vue";
import LoginDialog from "./components/Dialogs/LoginDialog.vue";
import UserDialog from "./components/Dialogs/UserDialog.vue";
import ShoppingCartDialog from "./components/Dialogs/ShoppingCartDialog.vue";

export default {
  name: "e-smd",
  components: {
    ProductList,
    AdminPage,
    LoginDialog,
    UserDialog,
    ShoppingCartDialog,
  },
  data: () => ({
    isLoginDialogVisible: false,
    isUserDialogVisible: false,
    isShoppingCartDialogVisible: false,
    user: { administrador: false },
    shoppingCartList: [],
    productList: [],
  }),
  computed: {
    productsTotal() {
      return this.shoppingCartList.reduce((sum, product) => {
        return sum + product.quantity;
      }, 0);
    },
  },
  methods: {
    loadProducts() {
      axios
        .get("http://localhost:8080/e-commerce/ListarProdutosEmEstoque")
        .then((response) => {
          this.productList = response.data;
        })
        .catch((error) => console.log(error));
    },
    onLogin(user) {
      this.user = user;
    },
    logout() {
      axios
        .delete("http://localhost:8080/e-commerce/Logout")
        .catch((error) => console.log(error))
        .finally(() => {
          this.user = { administrador: false };
        });
    },
    addToCart(product) {
      const index = this.shoppingCartList.findIndex(
        (cartProduct) => cartProduct.id == product.id
      );
      if (index > -1) {
        let quantity = this.shoppingCartList[index].quantity + product.quantity;
        if (quantity > product.quantidade) quantity = product.quantidade;
        if (quantity > 10) quantity = 10;
        this.shoppingCartList[index].quantity = quantity;
      } else {
        this.shoppingCartList.push(product);
      }
    },
    removeFromCart(index) {
      this.shoppingCartList.splice(index, 1);
    },
  },
};
</script>

<style>
#login-link {
  text-decoration: underline;
  cursor: pointer;
}
</style>