<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card>
      <v-card-title>Editar Categoria</v-card-title>

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
        <v-btn color="primary" @click="onEdit()" :disabled="!description">
          Editar
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";

export default {
  name: "EditCategoryDialog",
  props: {
    isDialogVisible: Boolean,
    selectedCategory: Object,
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
    selectedCategory(newValue) {
      this.description = newValue.descricaoCategoria;
    },
  },
  methods: {
    closeDialog() {
      this.isVisible = false;
    },
    onEdit() {
      const data =
        "id=" +
        this.selectedCategory.id +
        "&descricao_categoria=" +
        encodeURIComponent(this.description);

      axios
        .post("http://localhost:8081/e-commerce/AtualizarCategoria", data)
        .catch((error) => console.log(error))
        .finally(() => {
          this.$emit("reload-categories");
          this.closeDialog();
        });
    },
  },
};
</script>