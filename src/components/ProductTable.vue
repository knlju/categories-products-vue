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
      <th>Ukupna vrednost</th>
      <th span="2">Actions</th>
    </thead>
    <tr v-for="p in products" :key="p.productId">
      <Product
        :product="p"
        :suppliers="suppliers"
        @deleteProduct="(id) => $emit('deleteProduct', id)"
        @editProduct="newProductData => $emit('editProduct', newProductData)"
      />
    </tr>
  </table>
</template>

<script>
import Product from "./Product";

export default {
  name: "App",
  props: {
    products: Array,
    suppliers: Array,
  },
  components: {
    Product,
  },
};
</script>

<style scoped>
</style>
