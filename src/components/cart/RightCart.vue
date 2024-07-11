<template>
  <div
    class="container"
    :class="{
      hideContainer: !componentVisible
    }"
  >
    <el-button
      @click="toggleComponent"
      class="toggle-button"
      type="warning"
      color="black"
      :icon="ShoppingCart"
    >
      {{ componentVisible ? 'Hide >' : 'Show' }}
    </el-button>
    <h3 style="margin-top: 0px; text-decoration: underline">Shopping cart</h3>

    <div class="component-ElementsCart">
      <ElementsCart :products="myProducts"></ElementsCart>
    </div>
    <div class="component-ResumeCart">
      <ResumeCart :totalProducts="totalProducts" :totalPrice="totalPrice.toFixed(2)"></ResumeCart>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import ElementsCart from './ElementsCart.vue'
import ResumeCart from './ResumeCart.vue'
import { useCartStore } from '@/stores/products'
import { ShoppingCart } from '@element-plus/icons-vue'

const cartStore = useCartStore()

let totalProducts = ref(0)
let myProducts = ref(cartStore.myProducts)

const totalpriceStorage = JSON.parse(localStorage.getItem('totalPrice') || 'null')
let totalPrice = totalpriceStorage ? ref(totalpriceStorage) : ref(0)

totalProducts = computed(() => cartStore.totalProducts)

watch(cartStore, (newValue) => {
  totalPrice.value = 0
  newValue.myProducts.forEach((product: { totalPrice: number }) => {
    totalPrice.value += product.totalPrice
  })
  localStorage.setItem('totalPrice', JSON.stringify(totalPrice.value))
  if (newValue.totalProducts === 0) {
    myProducts.value = []
    totalPrice.value = 0
  } else {
    myProducts.value = cartStore.myProducts
  }
})

let componentVisible = ref(false)

const toggleComponent = () => {
  componentVisible.value = !componentVisible.value
}
</script>

<style scoped>
.container {
  position: absolute;
  top: 0;
  right: 0;
  width: 300px;
  height: 100%;
  background-color: #f4f2f2;
  box-shadow: 1px 1px 5px 1px black;
  padding: 10px;
  z-index: 999;
}
.hideContainer {
  right: -300px !important;
}
.toggle-button {
  margin-bottom: 10px;
  position: absolute;
  left: -105px;
}

.component-ElementsCart {
  height: calc(100% - 130px);
  overflow: auto;
}
</style>
