<template>
  <table>
  <caption>Add a product</caption>
    <thead>
      <th>Name</th>
      <th>Category</th>
      <th>Action</th>
    </thead>
    <tr>
      <td><input v-model="productName" /></td>
      <td>
        <select v-model="categoryId">
          <option v-for="c in categories" :key="c.categoryId" :value="c.categoryId">
            {{ c.categoryName }}
          </option>
        </select>
      </td>
      <td>
        <button @click="HandleAddNewProductClick">
          Save new product
        </button>
      </td>  
    </tr>
  </table>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
        categoryId: 0,
        productName: "",
    };
  },
  props: {
    categories: Array,
  },
  methods: {
    async HandleAddNewProductClick() {
      if(this.$categoryId === 0 || this.productName.trim() === ""){
        console.log("validation failed")
        return
      }
      const { categoryId, productName } = this
      this.$emit("addNewProduct", {categoryId, productName})
    },
  },
};
</script>

<style scoped>
</style>