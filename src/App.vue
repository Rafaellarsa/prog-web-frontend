<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-btn class="d-flex align-center" @click="isAdminPage = false" text>
        <v-icon aria-label="Logo e-smd" x-large>mdi-store</v-icon>
        <v-toolbar-title class="shrink mt-1 hidden-sm-and-down" contain>
          e-smd
        </v-toolbar-title>
      </v-btn>

      <v-spacer />
      <span class="mr-2">
        <b>Bem-vindo! :)</b> <br />
        <span id="login-link" @click="isLoginDialogVisible = true">
          Entre ou cadastre-se
        </span>
      </span>
      <v-btn href="" target="_blank" text>
        <v-icon aria-label="Carrinho de compras">mdi-cart</v-icon>
        0
      </v-btn>
      <v-btn v-if="isAdmin" text @click="isAdminPage = true">
        <v-icon aria-label="Engrenagem">mdi-cog</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <AdminPage v-if="isAdminPage">blabla</AdminPage>
      <ProductList v-else />
    </v-main>

    <LoginDialog
      :isDialogVisible="isLoginDialogVisible"
      @close-dialog="isLoginDialogVisible = false"
    />
  </v-app>
</template>

<script>
import LoginDialog from "./components/LoginDialog.vue";
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
    isAdmin: true,
    isAdminPage: false,
  }),
};
</script>

<style>
#login-link {
  text-decoration: underline;
  cursor: pointer;
}
</style>