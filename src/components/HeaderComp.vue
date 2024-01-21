<script setup></script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-8 py-8">
    <div class="flex items-center">
      <img src="/logo.png" alt="Logo" class="w-10 mr-4" />
      <div>
        <h2 class="text-xl font-bold uppercase">Vue Shop</h2>
        <p class="text-slate-400">Магазин кроссовок</p>
      </div>
    </div>

    <ul class="flex items-center gap-10">
      <li class="flex items-center gap-3 cursor-pointer text-slate-400 hover:text-black">
        <img src="/cart.svg" alt="Cart" />
        <b>1250 руб.</b>
      </li>

      <li class="flex items-center gap-3 cursor-pointer text-slate-400 hover:text-black">
        <img src="/heart.svg" alt="Favorite" />
        <span>Закладки</span>
      </li>

      <li class="flex items-center gap-3 cursor-pointer text-slate-400 hover:text-black">
        <img src="/profile.svg" alt="Profile" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>

<style scoped></style>
