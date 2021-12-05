<template>
  <td><input type="text" :value="product.productName" /></td>
  <td><input type="text" :value="product.supplierId" /></td>
  <td><input type="text" :value="product.quantityPerUnit" /></td>
  <td><input type="text" :value="product.unitPrice" /></td>
  <td><input type="text" :value="product.unitsInStock" /></td>
  <td><input type="text" :value="product.unitsOnOrder" /></td>
  <td><input type="text" :value="product.reorderLevel" /></td>
  <td><input type="checkbox" :value="product.discontinued" /></td>
  <td><button @click="HandleEditClick">edit</button></td>
  <td><button @click="HandleDeleteClick(product.productId)">delete</button></td>
</template>

<script>
export default {
  name: "App",
  data() {
    return {};
  },
  props: {
    product: Object,
  },
  methods: {
    HandleDeleteClick() {
      this.$emit("deleteProduct", this.product.productId);
    },
  },
  updated() {
    console.log("Product updated");
  },
};
</script>

<style scoped>
</style>
