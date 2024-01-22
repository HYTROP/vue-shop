<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

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

    <div v-if="!totalPrice" class="flex h-full items-center">
      <InfoBlock
        image-url="/package-icon.png"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
      />
    </div>

    <div v-else>
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
          class="w-full disabled:from-slate-400 disabled:to-slate-400 disabled:bg-slate-400 transition bg-lime-500 hover:bg-lime-600 bg-gradient-to-r active:from-lime-400 active:to-amber-300 opacity-100 cursor-pointer rounded-xl py-3 px-4 text-zinc-100 text-xl"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
