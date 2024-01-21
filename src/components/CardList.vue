<script setup>
import CardComp from './CardComp.vue'

defineProps({
  items: Array
})

const onClickAdd = () => {
  alert('Добавлено в корзину')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-4">
    <CardComp
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
