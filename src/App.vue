<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-icon aria-label="Logo e-smd" x-large>mdi-store</v-icon>
      </div>
      <v-toolbar-title class="shrink mt-1 hidden-sm-and-down" contain
        >e-smd</v-toolbar-title
      >

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
    </v-app-bar>

    <v-main>
      <ProductList />
    </v-main>

    <v-dialog v-model="isLoginDialogVisible" width="500">
      <v-card v-if="!isSignUpForm">
        <v-row no-gutters>
          <v-col cols="12" sm="6">
            <v-card-title>Quero criar uma conta</v-card-title>

            <v-card-text>
              <v-text-field
                label="Email"
                :rules="[requiredRule, emailRule]"
                hide-details="auto"
              />
            </v-card-text>

            <v-card-actions>
              <v-spacer />
              <v-btn color="primary" @click="isSignUpForm = true">
                Continuar
              </v-btn>
            </v-card-actions>
          </v-col>

          <v-divider vertical />

          <v-col cols="12" sm="6">
            <v-card-title>Já sou cliente</v-card-title>

            <v-card-text>
              <v-text-field
                label="Login"
                :rules="[requiredRule]"
                hide-details="auto"
              />
              <v-text-field
                label="Senha"
                :rules="[requiredRule]"
                type="password"
                hide-details="auto"
              />
            </v-card-text>

            <v-card-actions>
              <v-spacer />
              <v-btn color="primary" @click="isLoginDialogVisible = false">
                Entrar
              </v-btn>
            </v-card-actions>
          </v-col>
        </v-row>
      </v-card>

      <v-card v-else>
        <v-card-title>Criar uma conta</v-card-title>

        <v-card-text>
          <v-text-field
            label="Nome"
            :rules="[requiredRule]"
            hide-details="auto"
          />
          <v-text-field
            label="Endereço"
            :rules="[requiredRule]"
            hide-details="auto"
          />
          <v-text-field
            label="Login"
            :rules="[requiredRule]"
            hide-details="auto"
          />
          <v-text-field
            label="Senha"
            :rules="[requiredRule]"
            type="password"
            hide-details="auto"
          />
          <v-text-field
            label="Confirmar senha"
            :rules="[requiredRule]"
            type="password"
            hide-details="auto"
          />
        </v-card-text>

        <v-card-actions>
          <v-spacer />
          <v-btn color="primary" text @click="isSignUpForm = false">
            Voltar
          </v-btn>
          <v-btn color="primary" @click="isLoginDialogVisible = false">
            Cadastrar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import ProductList from "./components/ProductList.vue";

export default {
  name: "e-smd",

  components: {
    ProductList,
  },

  data: () => ({
    isLoginDialogVisible: false,
    isSignUpForm: false,
    requiredRule: (value) => !!value || "Obrigatório.",
    emailRule: (value) => {
      const pattern =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return pattern.test(value) || "Email inválido.";
    },
  }),
};
</script>

<style>
#login-link {
  text-decoration: underline;
  cursor: pointer;
}
</style>