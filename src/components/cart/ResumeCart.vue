<template>
  <div class="resumeBox">
    <div class="row">
      <h4 class="col">{{ totalProducts }} units</h4>
      <h4 class="col-5">{{ totalPrice }} €</h4>
    </div>
    <div>
      <el-button class="col-6" type="success">Buy</el-button>
      <el-button class="clearCart col-4" type="info" plain @click="clearCart()"
        >Clear Cart</el-button
      >
    </div>
  </div>
</template>

<script setup lang="ts">
import { useCartStore } from '@/stores/products'

defineProps<{
  totalProducts: number
  totalPrice: number
}>()

const cartStore = useCartStore()

const clearCart = async () => {
  await cartStore.clearCart()
  localStorage.clear()
}
</script>

<style scoped>
@import 'bootstrap/dist/css/bootstrap.css';
@import 'element-plus/dist/index.css';
.resumeBox {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
  padding: 10px;
  box-shadow: 1px 1px 5px 1px grey;
}
</style>
