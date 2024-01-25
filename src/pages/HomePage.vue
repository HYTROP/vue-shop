<script setup>
import axios from 'axios'
import { inject, reactive, watch, ref, onMounted } from 'vue'
import CardList from '../components/CardList.vue'

import { debounce } from 'lodash'

const { addToCart, removeFromCart, cart } = inject('cartActions')

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 350)

const onClickAddToCart = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://de475c8949732766.mokky.dev/items`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      favoriteId: null,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    console.error(error)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item
      }

      item.isFavorite = true

      const { data } = await axios.post(`https://de475c8949732766.mokky.dev/favorites`, obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false

      await axios.delete(`https://de475c8949732766.mokky.dev/favorites/${item.favoriteId}`)

      item.favoriteId = null
    }
  } catch (error) {
    console.error(error)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://de475c8949732766.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favoriteItem = favorites.find((obj) => obj.parentId === item.id)

      if (!favoriteItem) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favoriteItem.id
      }
    })
  } catch (error) {
    console.error(error)
  }
}

onMounted(async () => {
  const localStorageCart = localStorage.getItem('cart')
  cart.value = localStorageCart ? JSON.parse(localStorageCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filters, fetchItems)
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold">Все кроссовки</h2>

    <div class="flex items-center gap-4">
      <select @change="onChangeSelect" class="py-2 px-2 border rounded-md outline-none">
        <option value="name">По названию</option>
        <option value="price">По цене ( дешевые )</option>
        <option value="-price">По цене ( дорогие )</option>
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
  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddToCart" />
</template>
