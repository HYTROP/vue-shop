<script setup>
import CardComp from './CardComp.vue'

defineProps({
  items: Array,
  isFavorite: Boolean
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-4" v-auto-animate>
    <CardComp
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickAdd="() => emit('addToCart', item)"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
