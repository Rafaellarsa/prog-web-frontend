<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card>
      <v-card-title>Editar conta</v-card-title>

      <v-card-text>
        <v-text-field
          label="Nome"
          v-model="userInfo.nome"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Endereço"
          v-model="userInfo.endereco"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Login"
          v-model="userInfo.login"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Senha"
          v-model="userInfo.senha"
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
        <v-btn color="primary" text @click="isVisible = false"> Voltar </v-btn>
        <v-btn color="primary" @click="onEdit()" :disabled="isSignUpDisabled">
          Atualizar
        </v-btn>
      </v-card-actions>

      <v-card-text>
        <v-btn color="red" text block @click="onRemove()">
          <v-icon color="red" aria-label="Remover">mdi-delete</v-icon>Deletar
          conta
        </v-btn>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";

export default {
  name: "LoginDialog",
  props: {
    isDialogVisible: Boolean,
    user: Object,
  },
  data: () => ({
    isVisible: false,
    userInfo: {
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
        !this.userInfo.nome ||
        !this.userInfo.endereco ||
        !this.userInfo.email ||
        !this.userInfo.login ||
        !this.userInfo.senha ||
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
    user(newValue) {
      this.userInfo = newValue;
    },
  },
  methods: {
    onEdit() {
      if (this.userInfo.senha == this.passwordConfirmation) {
        let data = "";
        for (let [key, value] of Object.entries(this.userInfo)) {
          data += key + "=" + encodeURIComponent(value) + "&";
        }
        data = data.slice(0, -1);

        axios
          .put("http://localhost:8080/e-commerce/AtualizarCliente", data)
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
    onRemove() {
      axios
        .delete(
          `http://localhost:8080/e-commerce/DeletarCliente?id_produto=${this.userInfo.id}`
        )
        .then((response) => {
          console.log(response);
        })
        .catch((error) => console.log(error))
        .finally(() => {
          this.isVisible = false;
        });
    },
  },
};
</script>