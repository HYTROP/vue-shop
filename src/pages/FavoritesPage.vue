<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'

import CardList from '../components/CardList.vue'

const favorites = ref([])
onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://de475c8949732766.mokky.dev/favorites?_relations=items`
    )

    favorites.value = data.map((obj) => obj.item)
  } catch (error) {
    console.error(error)
  }
})
</script>

<template>
  <CardList :items="favorites" />
</template>
