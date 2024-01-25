<script setup>
import { computed, inject, ref } from 'vue'

import axios from 'axios'

import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const isCreatedOrder = ref(false)
const orderID = ref(null)

const props = defineProps({
  totalPrice: Number,
  taxesSum: Number
})

const { cart } = inject('cartActions')

const createOrder = async () => {
  try {
    isCreatedOrder.value = true

    const { data } = await axios.post(`https://de475c8949732766.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice
    })

    cart.value = []

    orderID.value = data.id
  } catch (error) {
    console.error(error)
  } finally {
    isCreatedOrder.value = false
  }
}

const cartIsEmpty = computed(() => {
  return cart.value.length === 0
})

const buttonDisabled = computed(() => isCreatedOrder.value || cartIsEmpty.value)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black/50 z-10" v-auto-animate></div>
  <div class="bg-white min-w-[386px] fixed top-0 right-0 h-full z-20 p-4 overflow-auto">
    <DrawerHead class="sticky h-20 -top-5 z-20 bg-white" />

    <div v-if="!totalPrice || orderID" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderID"
        image-url="/package-icon.png"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
      />
      <InfoBlock
        v-if="orderID"
        image-url="/order-success-icon.png"
        title="Заказ оформлен!"
        :description="`Ваш заказ №${orderID} скоро будет передан курьерской службе`"
      />
    </div>

    <div v-else class="flex flex-col max-w-[356px]">
      <CartItemList />

      <div class="sticky -bottom-4 w-[356px] bg-white opacity-100">
        <div class="flex flex-col gap-4 mb-6">
          <div class="flex gap-2">
            <span>Итого: </span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalPrice }} руб.</b>
          </div>

          <div class="flex gap-2">
            <span>Налог 5%: </span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ Math.round(taxesSum) }} руб.</b>
          </div>
        </div>
        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="w-full disabled:from-slate-400 disabled:to-slate-400 disabled:bg-slate-400 transition bg-lime-500 hover:bg-lime-600 bg-gradient-to-r active:from-lime-400 active:to-amber-300 opacity-100 cursor-pointer rounded-xl py-3 px-4 text-zinc-100 text-xl"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
