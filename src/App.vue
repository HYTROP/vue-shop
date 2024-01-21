<script setup>
import { onMounted, ref, watch, reactive } from 'vue'
import axios from 'axios'

import HeaderComp from './components/HeaderComp.vue'
import CardList from './components/CardList.vue'
import DrawerComp from './components/DrawerComp.vue'

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
      // searchQuery: filters.searchQuery
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://de475c8949732766.mokky.dev/items`, {
      params
    })
    items.value = data
  } catch (error) {
    console.error(error)
  }
}

onMounted(fetchItems)
watch(filters, fetchItems)
</script>

<template>
  <!-- <DrawerComp /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
    <HeaderComp />
    <div class="p-4">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold">Все кроссовки</h2>

        <div class="flex items-center gap-4">
          <select @change="onChangeSelect" class="py-2 px-2 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="flex items-center relative">
            <img src="/search.svg" alt="Search" class="absolute left-4 w-4" />
            <input
              @input="onChangeSearchInput"
              type="text"
              placeholder="Поиск..."
              class="border border-gray-300 rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-500"
            />
          </div>
        </div>
      </div>
    </div>

    <CardList :items="items" />
  </div>
</template>

<style scoped></style>
