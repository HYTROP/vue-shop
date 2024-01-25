<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'

import CardList from '../components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(`https://de475c8949732766.mokky.dev/favorites`)

    favorites.value = data.map((obj) => obj.item)
  } catch (error) {
    console.error(error)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold">Мои закладки</h2>
  <CardList :items="favorites" v-if="favorites.length" />
  <p v-else class="text-2xl">В избранном пока ничего нет</p>
</template>
