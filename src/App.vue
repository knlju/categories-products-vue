<template>
  <div :class="['main-wrapper', isLoading && 'no-scroll']">
    <div v-if="isLoading" class="loading-container">
      <div class="loading-spinner"></div>
    </div>
    <label>
      Select a Category
      <select @change="HandleSelectChange">
        <option :value="false">Select</option>
        <option v-for="c in categories" :key="c.categoryId" :value="c.categoryId">
          {{ c.categoryName }}
        </option>
      </select>
    </label>
    <product-table
      v-if="selectedCategory"
      :products="selectedCategory.products"
      :suppliers="suppliers"
      @deleteProduct="DeleteProductByID"
      @editProduct="EditProduct"
    />
    <add-a-product :suppliers="suppliers" :categories="categories" @addNewProduct="AddNewProduct" />
  </div>
</template>

<script>
import ProductTable from "./components/ProductTable.vue";
import axios from "axios";
import { API, CATEGORY_EP, PRODUCTS_EP, SUPPLIERS_EP } from "./utils";
import AddAProduct from "./components/AddAProduct.vue";

export default {
  name: "App",
  data() {
    return {
      categories: [],
      suppliers: [],
      isLoading: false,
      selectedCategory: false,
    };
  },
  components: {
    ProductTable,
    AddAProduct,
  },
  mounted: async function () {
    await this.FetchData();
  },
  methods: {
    async FetchData() {
      this.isLoading = true;
      const categoriesResponse = await axios.get(`${API}/${CATEGORY_EP}`);
      const productsResponse = await axios.get(`${API}/${PRODUCTS_EP}`);
      const suppliersResponse = await axios.get(`${API}/${SUPPLIERS_EP}`);
      const copiedCategories = categoriesResponse.data.map((c) => ({
        ...c,
        products: [],
      }));
      productsResponse.data.forEach((p) => {
        copiedCategories.find((c) => c.categoryId === p.categoryId)?.products.push(p)
        if(p.supplierId) {
          p.supplier = suppliersResponse.data.find(s => s.supplierId === p.supplierId)
        }
      }
      );
      this.categories = copiedCategories;
      this.selectedCategory =
        copiedCategories.find(
          (category) => category.categoryId === this.selectedCategory.categoryId
        ) || false;
      this.suppliers = suppliersResponse.data
      this.isLoading = false;
    },
    HandleSelectChange(e) {
      const categoryId = parseInt(e.target.value);
      this.selectedCategory = this.categories.find(
        (c) => c.categoryId === categoryId
      );
    },
    async DeleteProductByID(productId) {
      this.isLoading = true;
      const response = await axios.delete(`${API}/${PRODUCTS_EP}/${productId}`);
      console.log("delete");
      console.log(response);
      await this.FetchData();
      this.isLoading = false;
    },
    async AddNewProduct(newProduct) {
      this.isLoading = true;
      console.log(newProduct)
      const response = await axios.post(`${API}/${PRODUCTS_EP}`, newProduct);
      console.log("AddNewProduct");
      console.log(response);
      await this.FetchData();
      this.isLoading = false;
    },
    async EditProduct(newProductData) {
      this.isLoading = true;
      const response = await axios.put(`${API}/${PRODUCTS_EP}/${newProductData.productId}`, newProductData);
      console.log("EditProduct");
      console.log(response);
      await this.FetchData();
      this.isLoading = false;
      console.log(newProductData)
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.main-wrapper {
  max-height: 100vh;
  overflow: auto;
}
.loading-container {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.3);
}
.loading-spinner {
  height: 100px;
  width: 100px;
  border-radius: 50%;
  border: 10px solid #333;
  border-top: 10px solid #ddd;
  animation: spinner 2s infinite;
}
.no-scroll {
  overflow: hidden;
}
@keyframes spinner {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
