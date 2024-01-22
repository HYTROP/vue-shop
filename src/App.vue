<script setup>
import { onMounted, ref, watch, reactive, provide, computed } from 'vue'
import axios from 'axios'

import HeaderComp from './components/HeaderComp.vue'
import CardList from './components/CardList.vue'
import DrawerComp from './components/DrawerComp.vue'

const items = ref([])
const cart = ref([])
const isCreatedOrder = ref(false)
const drawerOpen = ref(false)

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const totalPriceSum = () => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
}
const taxesSum = () => {
  return totalPriceSum() * 0.05
}

const cartIsEmpty = computed(() => {
  return cart.value.length === 0
})

const cartButtonDisabled = computed(() => isCreatedOrder.value || cartIsEmpty.value)

const openDrawer = () => {
  drawerOpen.value = true
}
const closeDrawer = () => {
  drawerOpen.value = false
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = () => {
  try {
    isCreatedOrder.value = true
    const { data } = axios.post(`https://de475c8949732766.mokky.dev/orders`, {
      items: cart.value,
      totalPriceSum: totalPriceSum.value
    })

    cart.value = []

    return data
  } catch (error) {
    console.error(error)
  } finally {
    isCreatedOrder.value = false
  }
}

const onClickAddToCart = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://de475c8949732766.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favoriteItem = favorites.find((favoriteItem) => favoriteItem.favId === item.id)

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

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = { favId: item.id }

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
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (error) {
    console.error(error)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
provide('cartActions', {
  cart,
  closeDrawer,
  openDrawer,
  removeFromCart,
  addToCart
})
</script>

<template>
  <DrawerComp
    v-if="drawerOpen"
    :total-price="totalPriceSum() + taxesSum()"
    :taxes-sum="taxesSum()"
    @create-order="createOrder()"
    :button-disabled="cartButtonDisabled"
  />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
    <!-- Header -->

    <HeaderComp :total-price="totalPriceSum()" @open-drawer="openDrawer" />

    <div class="p-4">
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
    </div>

    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddToCart" />
  </div>
</template>

<style scoped></style>
