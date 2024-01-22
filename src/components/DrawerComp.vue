<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'

const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number,
  taxesSum: Number,
  buttonDisabled: Boolean
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
        :disabled="buttonDisabled"
        @click="emit('createOrder')"
        class="w-full disabled:from-slate-400 disabled:to-slate-400 disabled:bg-slate-400 bg-gradient-to-r from-lime-400 to-green-400 hover:from-emerald-700 hover:to-emerald-400 active:emerald-400 active:to-amber-300 opacity-50 cursor-pointer rounded-xl py-3 px-4 text-zinc-100 text-xl"
      >
        {{ totalPrice ? 'Оформить заказ' : 'Добавьте хотя бы одну пару кроссовок' }}
      </button>
    </div>
  </div>
</template>
