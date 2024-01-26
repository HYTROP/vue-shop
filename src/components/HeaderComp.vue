<script setup>
const emit = defineEmits(['openDrawer'])

defineProps({
  totalPrice: Number
})
</script>

<template>
  <header
    class="lg:flex md:flex justify-between border-b border-slate-300 px-8 py-8 sm:px-4 sm:py-4"
  >
    <router-link to="/">
      <div class="flex items-center sm:m-4">
        <img src="/logo.png" alt="Logo" class="w-10 mr-4" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Shop</h2>
          <p class="text-slate-400">Магазин кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class="lg:flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex mb-2 items-center gap-3 cursor-pointer text-slate-400 hover:text-black"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} руб.</b>
      </li>

      <router-link to="/favorites">
        <li class="flex mb-2 items-center gap-3 cursor-pointer text-slate-400 hover:text-black">
          <img src="/heart.svg" alt="Favorite" />
          <span>Закладки</span>
        </li>
      </router-link>

      <li class="flex items-center gap-3 cursor-pointer text-slate-400 hover:text-black">
        <img src="/profile.svg" alt="Profile" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>

<style scoped></style>
