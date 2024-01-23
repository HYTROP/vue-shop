<script setup>
import { watch, ref, provide, computed } from 'vue'
import axios from 'axios'

import HeaderComp from './components/HeaderComp.vue'
import DrawerComp from './components/DrawerComp.vue'

const cart = ref([])
const isCreatedOrder = ref(false)

const drawerOpen = ref(false)

const totalPriceSum = () => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
}
const taxesSum = () => {
  return totalPriceSum() * 0.05
}

const cartIsEmpty = computed(() => {
  return cart.value.length === 0
})

const cartButtonDisabled = computed(() => isCreatedOrder.value || cartIsEmpty.value)

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

const createOrder = () => {
  try {
    isCreatedOrder.value = true
    const { data } = axios.post(`https://de475c8949732766.mokky.dev/orders`, {
      items: cart.value,
      totalPriceSum: totalPriceSum.value
    })

    cart.value = []

    return data
  } catch (error) {
    console.error(error)
  } finally {
    isCreatedOrder.value = false
  }
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
  <DrawerComp
    v-if="drawerOpen"
    :total-price="totalPriceSum() + taxesSum()"
    :taxes-sum="taxesSum()"
    @create-order="createOrder()"
    :button-disabled="cartButtonDisabled"
  />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
    <!-- Header -->

    <HeaderComp :total-price="totalPriceSum()" @open-drawer="openDrawer" />

    <div class="p-4">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
