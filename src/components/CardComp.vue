<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="relative m-4 border border-gray-100 bg-white rounded-3xl p-4 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      @click="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like 1"
      class="absolute top-4 left-4"
    />
    <img :src="imageUrl" alt="Sneakers" class="max-h-[170px]" />

    <p class="mt-6">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">ЦЕНА:</span>
        <b>{{ price }} руб.</b>
      </div>
      <img @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" />
    </div>
  </div>
</template>

<style scoped></style>
