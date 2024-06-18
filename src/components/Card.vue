<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="relative bg-white border border-slatet-100 rounded-3xl px-8 py-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      v-if="onClickFavorite"
      class="absolute top-4 left-4"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt=""
      @click="onClickFavorite"
    />
    <img class="mt-4" :src="imageUrl" alt="" />
    <p>{{ title }}</p>
    <div class="flex justify-between mt-2x">
      <div class="flex flex-col">
        <span>Price:</span>
        <b>{{ price }}</b>
      </div>
      <img @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" />
    </div>
  </div>
</template>
