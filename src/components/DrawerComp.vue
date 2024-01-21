<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'

defineProps({
  totalPrice: Number,
  taxesSum: Number
})
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black/50 z-10"></div>
  <div class="bg-white min-w-[386px] fixed top-0 right-0 h-full z-20 p-4">
    <DrawerHead />

    <CartItemList />

    <div class="fixed bottom-8 w-[356px]">
      <div class="flex flex-col gap-4 mb-6">
        <div class="flex gap-2">
          <span>Итого: </span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ Math.round(totalPrice) }} руб.</b>
        </div>

        <div class="flex gap-2">
          <span>Налог 5%: </span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ Math.round(taxesSum) }} руб.</b>
        </div>
      </div>
      <button
        disabled=""
        class="w-full bg-gradient-to-r from-lime-500 to-green-500 hover:from-lime-600 hover:to-green-600 active:from-lime-700 active:to-green-700 disabled:bg-slate-300 pointer-events-none opacity-50 cursor-pointer rounded-xl py-3 px-4 text-white"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
