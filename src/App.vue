<script setup>
import { watch, ref, provide } from 'vue'

import HeaderComp from './components/HeaderComp.vue'
import DrawerComp from './components/DrawerComp.vue'

const cart = ref([])

const drawerOpen = ref(false)

const totalPrice = () => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
}

const taxesSum = () => {
  return totalPrice() * 0.05
}

const openDrawer = () => {
  drawerOpen.value = true
}
const closeDrawer = () => {
  drawerOpen.value = false
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cartActions', {
  cart,
  closeDrawer,
  openDrawer,
  removeFromCart,
  addToCart
})
</script>

<template>
  <DrawerComp v-if="drawerOpen" :total-price="totalPrice() + taxesSum()" :taxes-sum="taxesSum()" />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
    <!-- Header -->

    <HeaderComp :total-price="totalPrice()" @open-drawer="openDrawer" />

    <div class="sm:grid-auto-flow p-4">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
