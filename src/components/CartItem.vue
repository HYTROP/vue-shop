<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  price: Number,
  imageUrl: String
})
</script>

<template>
  <div class="flex items-center gap-4 border border-slate-100 p-4 mb-4 rounded-xl">
    <img class="w-16" :src="imageUrl" :alt="title" />

    <div class="flex flex-col flex-1">
      <p class="max-w-[240px]">{{ title }}</p>
      <div class="flex justify-between">
        <b>{{ price }} руб.</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-50 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
        />
      </div>
    </div>
  </div>
</template>
