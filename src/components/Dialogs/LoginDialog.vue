<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card v-if="!isSignUpForm">
      <v-row no-gutters>
        <v-col cols="12" sm="6">
          <v-card-title>Quero criar uma conta</v-card-title>

          <v-card-text>
            <v-text-field
              label="Email"
              v-model="signUpInfo.email"
              :rules="[requiredRule, emailRule]"
              hide-details="auto"
            />
          </v-card-text>

          <v-card-actions>
            <v-spacer />
            <v-btn
              color="primary"
              @click="isSignUpForm = true"
              :disabled="!signUpInfo.email"
            >
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
              v-model="loginInfo.login"
              :rules="[requiredRule]"
              hide-details="auto"
            />
            <v-text-field
              label="Senha"
              v-model="loginInfo.senha"
              :rules="[requiredRule]"
              type="password"
              hide-details="auto"
            />
          </v-card-text>

          <v-card-actions>
            <v-spacer />
            <v-btn
              color="primary"
              @click="onLogin()"
              :disabled="!(loginInfo.login && loginInfo.senha)"
            >
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
          v-model="signUpInfo.nome"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Endereço"
          v-model="signUpInfo.endereco"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Login"
          v-model="signUpInfo.login"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Senha"
          v-model="signUpInfo.senha"
          :rules="[requiredRule]"
          type="password"
          hide-details="auto"
        />
        <v-text-field
          label="Confirmar senha"
          v-model="passwordConfirmation"
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
        <v-btn color="primary" @click="onSignUp()" :disabled="isSignUpDisabled">
          Cadastrar
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";

export default {
  name: "LoginDialog",
  props: {
    isDialogVisible: Boolean,
  },
  data: () => ({
    isVisible: false,
    isSignUpForm: false,
    loginInfo: {
      login: "",
      senha: "",
    },
    signUpInfo: {
      nome: "",
      endereco: "",
      email: "",
      login: "",
      senha: "",
    },
    passwordConfirmation: "",
    requiredRule: (value) => !!value || "Obrigatório.",
    emailRule: (value) => {
      const pattern =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return pattern.test(value) || "Email inválido.";
    },
  }),
  computed: {
    isSignUpDisabled() {
      return (
        !this.signUpInfo.nome ||
        !this.signUpInfo.endereco ||
        !this.signUpInfo.email ||
        !this.signUpInfo.login ||
        !this.signUpInfo.senha ||
        !this.passwordConfirmation
      );
    },
  },
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
    onLogin() {
      let data = "";
      for (let [key, value] of Object.entries(this.loginInfo)) {
        data += key + "=" + encodeURIComponent(value) + "&";
      }
      data = data.slice(0, -1);

      axios
        .post("http://localhost:8080/e-commerce/LoginCliente", data)
        .then((response) => {
          const user = response.data.usuario;
          this.$emit("login", user);
        })
        .catch((error) => console.log(error))
        .finally(() => {
          this.isVisible = false;
        });
    },
    onSignUp() {
      if (this.signUpInfo.senha == this.passwordConfirmation) {
        let data = "";
        for (let [key, value] of Object.entries(this.signUpInfo)) {
          data += key + "=" + encodeURIComponent(value) + "&";
        }
        data = data.slice(0, -1);

        axios
          .post("http://localhost:8080/e-commerce/NovoCliente", data)
          .then((response) => {
            console.log(response);
          })
          .catch((error) => console.log(error))
          .finally(() => {
            this.isVisible = false;
          });
      } else {
        console.log("As senhas devem coincidir");
      }
    },
  },
};
</script>