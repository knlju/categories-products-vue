<template>
  <td><input :class="[isEdit && 'editable']" type="text" v-model="productState.productName" :readonly=" !isEdit ? 'readonly' : false " /></td>
  <td>
    <select v-model="productState.supplierId" :disabled="!isEdit">
      <option v-for="s in suppliers" :value="s.supplierId" :key="s.supplierId">{{ s.companyName }}</option>
    </select>
  </td>
  <td><input :class="[isEdit && 'editable']" type="text" v-model="productState.quantityPerUnit" :readonly=" !isEdit ? 'readonly' : false " /></td>
  <td><input :class="[isEdit && 'editable']" type="number" v-model.number="productState.unitPrice" :readonly=" !isEdit ? 'readonly' : false " /></td>
  <td><input :class="[isEdit && 'editable']" type="number" v-model.number="productState.unitsInStock" :readonly=" !isEdit ? 'readonly' : false " /></td>
  <td><input :class="[isEdit && 'editable']" type="number" v-model.number="productState.unitsOnOrder" :readonly=" !isEdit ? 'readonly' : false " /></td>
  <td><input :class="[isEdit && 'editable']" type="number" v-model.number="productState.reorderLevel" :readonly=" !isEdit ? 'readonly' : false " /></td>
  <td><input :class="[isEdit && 'editable']" type="checkbox" v-model="productState.discontinued" :disabled=" !isEdit ? true : false " /></td>
  <td><input type="text" :value="Ukupno" readonly="readonly" /></td>
  <td>
    <button v-if="!isEdit" @click="HandleEditClick">edit</button>
    <button v-else @click="HandleSaveClick">save</button>
  </td>
  <td><button @click="HandleDeleteClick">delete</button></td>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      productState: {},
      isEdit: false,
    };
  },
  props: {
    product: Object,
    suppliers: Array
  },
  mounted() {
    this.productState = {...this.product}
  },
  methods: {
    HandleDeleteClick() {
      this.$emit("deleteProduct", this.product.productId);
    },
    HandleEditClick() {
      this.isEdit = true
    },
    HandleSaveClick() {
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

      this.isEdit = false
      this.$emit("editProduct", this.productState)
    }
  },
  computed: {
    Ukupno() {
      return this.productState?.unitPrice * this.productState?.unitsInStock
    }
  },
};
</script>

<style scoped>
input {
  border: none;
  outline: none;
  box-sizing: border-box;
}
.editable {
  border: 1px solid black;
}
.editable:focus {
  outline: 1px solid black;
}
</style>
