<template>
  <div>
    <div v-for="(product, index) in products" :key="index">
      <div class="product-item">
        <div class="product-values">
          <img :src="product.thumbnail" :alt="product.title" class="product-image col" />
          <div class="product-actions">
            <el-input-number
              v-model="product.quantity"
              :step="1"
              step-strictly
              :min="1"
              @change="quantityChanged(product)"
            /><el-button
              class="removeProduct"
              size="small"
              @click="removeProduct(product)"
              type="danger"
              plain
              :icon="Delete"
              >Remove</el-button
            >
            <h5>{{ calcTotalPrice(product) }} € (-{{ product.discountPercentage }}%)</h5>
          </div>
        </div>

        <div class="product-info">
          <h4 class="col title">
            {{ product.title }}
          </h4>
          <p class="col description">
            {{ product.description }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useCartStore } from '@/stores/products'
import { Delete } from '@element-plus/icons-vue'

defineProps<{
  products?: Array<{
    price: number
    quantity: number
    totalPrice?: number
    thumbnail: string | undefined
    title: string | undefined
    discountPercentage: any
    description: any
  }>
}>()

const cartStore = useCartStore()

const calcTotalPrice = (product: { price: number; quantity: number; totalPrice?: number }) => {
  return (product.totalPrice = product.price * product.quantity)
}

const removeProduct = (product: { price: number; quantity: number; totalPrice?: number }) => {
  cartStore.removeProduct(product)
  localStorage.setItem('cartProducts', JSON.stringify(cartStore.myProducts))
}

const quantityChanged = (product: { price: number; quantity: number; totalPrice?: number }) => {
  cartStore.quantityChanged(product)
  localStorage.setItem('cartProducts', JSON.stringify(cartStore.myProducts))
}
</script>

<style scoped>
.product-item {
  display: flex;
  flex-direction: column;
  align-items: left;
  margin-bottom: 10px;
}

.product-image {
  width: 100px;
  height: 100px;
  object-fit: cover;
  margin-right: 10px;
}
.product-info {
  text-align: left;
  text-overflow: ellipsis;
  height: 150px;
  overflow: hidden;
}
.product-values {
  display: flex;
  flex-direction: row;
}

.product-actions button {
  margin-right: 5px;
}
</style>
