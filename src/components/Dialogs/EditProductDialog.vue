<template>
  <v-dialog v-model="isVisible" width="500">
    <v-card>
      <v-card-title>Editar Produto</v-card-title>

      <v-card-text>
        <v-text-field
          label="Nome"
          v-model="productInfo.nome"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Descrição"
          v-model="productInfo.descricao"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Preço"
          v-model="productInfo.preco"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Foto"
          v-model="productInfo.foto"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Quantidade"
          v-model="productInfo.qntde"
          :rules="[requiredRule]"
          hide-details="auto"
        />
        <v-text-field
          label="Categoria"
          v-model="productInfo.id_categoria"
          :rules="[requiredRule]"
          hide-details="auto"
        />
      </v-card-text>

      <v-divider />

      <v-card-actions>
        <v-spacer />
        <v-btn color="primary" text @click="closeDialog()"> Cancelar </v-btn>
        <v-btn
          color="primary"
          @click="onEdit()"
          :disabled="isEditButtonDisabled"
        >
          Editar
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";

export default {
  name: "EditProductDialog",
  props: {
    isDialogVisible: Boolean,
    selectedProduct: Object,
  },
  data: () => ({
    isVisible: false,
    productInfo: {
      nome: "",
      descricao: "",
      preco: "",
      foto: "",
      qntde: "",
      id_categoria: "",
    },
    requiredRule: (value) => !!value || "Obrigatório.",
  }),
  computed: {
    isEditButtonDisabled() {
      return (
        !this.productInfo.nome ||
        !this.productInfo.descricao ||
        !this.productInfo.preco ||
        !this.productInfo.foto ||
        !this.productInfo.qntde ||
        !this.productInfo.id_categoria
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
    selectedProduct(newValue) {
      this.productInfo = newValue;
      this.productInfo.qntde = newValue.quantidade;
    },
  },
  methods: {
    closeDialog() {
      this.isVisible = false;
    },
    onEdit() {
      let data = "";
      for (let [key, value] of Object.entries(this.productInfo)) {
        data += key + "=" + encodeURIComponent(value) + "&";
      }
      data = data.slice(0, -1);

      axios
        .post("http://localhost:8080/e-commerce/AtualizarProduto", data)
        .catch((error) => console.log(error))
        .finally(() => {
          this.$emit("reload-products");
          this.closeDialog();
        });
    },
  },
};
</script>