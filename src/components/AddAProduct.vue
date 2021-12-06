<template>
  <table>
  <caption>Add a product</caption>
    <thead>
      <th>Product name</th>
      <th>Category</th>
      <th>Supplier</th>
      <th>Quantity</th>
      <th>Price</th>
      <th>In stock</th>
      <th>On order</th>
      <th>Reorder level</th>
      <th>Discounted</th>
      <th>Actions</th>
    </thead>
    <tr>
      <td><input type="text" v-model="productState.productName" /></td>
      <td>
        <select v-model="productState.categoryId">
          <option v-for="c in categories" :key="c.categoryId" :value="c.categoryId">
            {{ c.categoryName }}
          </option>
        </select>
      </td>
      <td>
        <!-- <input type="text" v-model="productState.supplierId" /> -->
        <select v-model="productState.supplierId">
          <option v-for="s in suppliers" :value="s.supplierId" :key="s.supplierId">{{ s.companyName }}</option>
        </select>
      </td>
      <td><input type="text" v-model="productState.quantityPerUnit" /></td>
      <td><input type="text" v-model="productState.unitPrice" /></td>
      <td><input type="text" v-model="productState.unitsInStock" /></td>
      <td><input type="text" v-model="productState.unitsOnOrder" /></td>
      <td><input type="text" v-model="productState.reorderLevel" /></td>
      <td><input type="checkbox" v-model="productState.discontinued" /></td>
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
        productState: {
          productName: "",
          supplierId: null,
          quantityPerUnit: "",
          unitPrice: 0,
          unitsInStock: 0,
          unitsOnOrder: 0,
          reorderLevel: 0,
          discontinued: false,
          categoryId: 0,
        }
    };
  },
  props: {
    categories: Array,
    suppliers: Array
  },
  methods: {
    async HandleAddNewProductClick() {
      if(this.productState.productName.trim() === ""){
        alert("product name cant be empty!!")
        return
      }
      
      if(!this.productState.unitPrice || this.productState.unitPrice === 0){
        alert("unit price can't be empty or zero!!")
        return
      }
      
      if(!this.productState.unitsInStock || this.productState.unitsInStock === 0){
        alert("units in stock can't be empty or zero!!")
        return
      }

      if(this.productState.categoryId === 0) {
        alert("please select a category!!")
        return
      }
      // if(this.$categoryId === 0 || this.productName.trim() === ""){
      //   console.log("validation failed")
      //   return
      // }
      // const { categoryId, productName } = this
      // this.$emit("addNewProduct", {categoryId, productName})
      this.$emit("addNewProduct", this.productState)
    },
  },
};
</script>

<style scoped>
</style>