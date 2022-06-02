<script>
import { mapState, mapStores, mapActions } from "pinia";
import { useProductStore } from "@/stores/product";

export default {
  data() {
    return {
      currentProduct: {
        id: "",
        description: "",
      },
      editing: false,
    };
  },
  computed: {
    ...mapStores(useProductStore),
    ...mapState(useProductStore, ["products"]),
  },
  methods: {
    ...mapActions(useProductStore, [
      "getAllProducts",
      "saveProduct",
      "deleteProduct",
    ]),
    async save() {
      try {
        const msg = await this.saveProduct(this.currentProduct);
        alert(msg);
        this.editing = false;
        this.currentProduct = {};
      } catch (e) {
        alert("Ooops! Algo de errado!");
      }
    },
    async deleteItem(product_id) {
      try {
        await this.deleteProduct(product_id);
        alert("Item excluído com sucesso.");
      } catch (e) {
        alert(e);
      }
    },
    prepareToUpdate(Product) {
      Object.assign(this.currentProduct, Product);
      this.editing = true;
    },
  },
  async mounted() {
    try {
      await this.getAllProducts();
    } catch (e) {
      alert(e);
    }
  },
};
</script>
<template>
  <h1>Cadastro de Categorias</h1>
  <div class="Product-form">
    <input type="text" v-model="currentProduct.name" />
    <input type="text" v-model="currentProduct.categoryId" />
    <button @click="save">
      {{ editing ? "Salvar" : "Adicionar" }}
    </button>
  </div>
  <div class="Product-list">
    <table class="table">
      <thead>
        <tr>
          <th class="text-left">
            <span> <h2>ID</h2> </span>
          </th>
          <th class="text-left">
            <span> <h2>Nome</h2> </span>
          </th>
          <th class="text-left">
            <span> <h2>Categoria ID</h2> </span>
          </th>
          <th class="text-left">
            <span> <h2>Ações</h2> </span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="Product of products" :key="Product.id">
          <td>{{ Product.id }}</td>
          <td>{{ Product.name }}</td>
          <td>{{ Product.categoryId }}</td>
          <td>
            <button @click="prepareToUpdate(Product)">Update</button>
            <button @click="deleteItem(Product.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
.Product-form input {
  width: 55%;
  height: 40px;
  border-radius: 20px;
  border: 1px solid gray;
  padding-left: 20px;
  font-size: 1.2em;
}

.Product-form button {
  height: 35px;
  width: 40%;
  margin-left: 2%;
  background-color: rgb(63, 63, 125);
  color: whitesmoke;
  font-size: 1.2em;
  border-radius: 20px;
  border: 0;
}

.Product-list,
.Product-form {
  margin: 3% auto;
  width: 70%;
  display: flex;
}
table {
  /* display: table; */
  border-collapse: separate;
  border-spacing: 2px;
  border-color: gray;
  width: 100%;
}

th {
  border-bottom: 2px solid #444;
  text-align: left;
}

td {
  padding: 10px;
}

thead tr {
  background-color: #444;
  color: whitesmoke;
}

tbody tr:nth-child(odd) {
  background-color: #c3c3c3;
}
</style>
