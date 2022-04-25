<template>
  <v-dialog v-model="isVisible" width="500">
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
            <v-btn color="primary" @click="isVisible = false"> Entrar </v-btn>
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
        <v-btn color="primary" @click="isVisible = false"> Cadastrar </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "LoginDialog",
  props: {
    isDialogVisible: Boolean,
  },
  data: () => ({
    isVisible: false,
    isSignUpForm: false,
    requiredRule: (value) => !!value || "Obrigatório.",
    emailRule: (value) => {
      const pattern =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return pattern.test(value) || "Email inválido.";
    },
  }),
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
};
</script>