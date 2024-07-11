<template>
  <div class="VueTest">
    <ProductsList :products="store.products" :totalProducts="store.totalProducts" class="col" />
    <RightCart class="col" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import ProductsList from './tables/ProductsList.vue'
import RightCart from './cart/RightCart.vue'
import { useProductList } from '@/stores/products'

export default defineComponent({
  name: 'VueTest',
  components: {
    ProductsList,
    RightCart
  },
  data() {
    return {
      store: useProductList()
    }
  },
  async created() {
    await this.store.getProductsData()
  }
})
</script>

<style scoped>
.VueTest {
  overflow: hidden;
}
</style>
