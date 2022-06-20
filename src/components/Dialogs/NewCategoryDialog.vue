<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card>
      <v-card-title>Nova Categoria</v-card-title>

      <v-card-text>
        <v-text-field
          label="Descrição"
          v-model="description"
          :rules="[requiredRule]"
          hide-details="auto"
        />
      </v-card-text>

      <v-divider />

      <v-card-actions>
        <v-spacer />
        <v-btn color="primary" text @click="closeDialog()"> Cancelar </v-btn>
        <v-btn color="primary" @click="onCreate()" :disabled="!description">
          Criar
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";

export default {
  name: "NewCategoryDialog",
  props: {
    isDialogVisible: Boolean,
  },
  data: () => ({
    isVisible: false,
    description: "",
    requiredRule: (value) => !!value || "Obrigatório.",
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
  methods: {
    closeDialog() {
      this.isVisible = false;
    },
    onCreate() {
      const data = "descricao=" + encodeURIComponent(this.description);

      axios
        .post("http://localhost:8080/e-commerce/NovaCategoria", data)
        .then((response) => {
          console.log(response);
        })
        .catch((error) => console.log(error))
        .finally(() => {
          this.closeDialog();
        });
    },
  },
};
</script>