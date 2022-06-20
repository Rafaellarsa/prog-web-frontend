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
          >Bem-vindo<span v-if="username">, {{ username }}</span
          >! :)</b
        >
        <br />
        <span
          v-if="!username"
          id="login-link"
          @click="isLoginDialogVisible = true"
        >
          Entre ou cadastre-se
        </span>
        <span v-else id="login-link" @click="logout()"> Sair </span>
      </span>
      <v-btn v-if="!isAdmin" href="" target="_blank" text>
        <v-icon aria-label="Carrinho de compras">mdi-cart</v-icon>
        0
      </v-btn>
    </v-app-bar>

    <v-main>
      <AdminPage v-if="isAdmin">blabla</AdminPage>
      <ProductList v-else />
    </v-main>

    <LoginDialog
      :isDialogVisible="isLoginDialogVisible"
      @close-dialog="isLoginDialogVisible = false"
      @login="onLogin"
    />
  </v-app>
</template>

<script>
import axios from "axios";

import LoginDialog from "./components/Dialogs/LoginDialog.vue";
import ProductList from "./components/ProductList.vue";
import AdminPage from "./components/AdminPage.vue";

export default {
  name: "e-smd",
  components: {
    AdminPage,
    ProductList,
    LoginDialog,
  },

  data: () => ({
    isLoginDialogVisible: false,
    isAdmin: false,
    username: null,
  }),
  methods: {
    onLogin(username, isAdmin) {
      this.username = username;
      this.isAdmin = isAdmin;
    },
    logout() {
      axios
        .delete("http://localhost:8080/e-commerce/Logout")
        .then((response) => {
          console.log(response);
        })
        .catch((error) => console.log(error))
        .finally(() => {
          this.username = null;
          this.isAdmin = false;
        });
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