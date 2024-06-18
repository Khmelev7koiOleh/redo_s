<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer'])
</script>
<template>
  <header class="flex justify-between px-8 py-8">
    <RouterLink to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div>
          <h1 class="text-xl font-bold uppercase">Vue Laptops</h1>
          <p>Best laptops store</p>
        </div>
      </div>
    </RouterLink>

    <ul class="flex items-center text-center gap-10">
      <li @click="() => emit('openDrawer')" class="flex text-gray gap-3">
        <img src="/cart.svg" alt="Cart" />
        <p>{{ totalPrice }}</p>
      </li>

      <RouterLink to="/favorites">
        <div class="flex gap-4">
          <li class="flex items-center text-gray gap-3">
            <img src="/heart.svg" alt="Heart" />
            <p>Favorites</p>
          </li>
          <li class="flex items-center text-gray gap-3">
            <img src="/profile.svg" alt="Profile" />
            <p>Profile</p>
          </li>
        </div></RouterLink
      >
    </ul>
  </header>
</template>
