<template>
  <table>
    <caption>
      Products
    </caption>
    <thead>
      <th>Product name</th>
      <th>Supplier</th>
      <th>Quantity</th>
      <th>Price</th>
      <th>In stock</th>
      <th>On order</th>
      <th>Reorder level</th>
      <th>Discounted</th>
      <th span="2">Actions</th>
    </thead>
    <tr v-for="p in products" :key="p.productId">
      <Product
        :product="p"
        @deleteProduct="(id) => $emit('deleteProduct', id)"
      />
    </tr>
  </table>
</template>

<script>
import Product from "./Product";

export default {
  name: "App",
  data() {
    return {};
  },
  props: {
    products: Array,
  },
  components: {
    Product,
  },
  updated() {
    console.log("ProductTable updated");
  },
};
</script>

<style scoped>
</style>
