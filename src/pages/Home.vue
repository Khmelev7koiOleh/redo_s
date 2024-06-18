<template>
  <div class="flex justify-between items-center mb-8 p-4">
    <h2 class="text-2xl font-bold p-4">All laptops</h2>
    <div class="flex gap-8">
      <select @change="onChangeSelect" class="border px-3" name="" id="">
        <option value="name">expensive</option>
        <option value="price">cheap</option>
        <option value="-price">favorable</option>
      </select>
      <div class="relative">
        <img class="absolute left-1 top-2" src="/search.svg" alt="" />
        <input
          @input="onChangeSearchInput"
          placeholder="Search"
          class="border rounded-l px-6 py-1"
          type="text"
        />
      </div>
    </div>
  </div>
  <div class="flex justify-between items-center">
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
  </div>
</template>
<script setup>
import axios from 'axios'
import { onMounted, ref, watch, reactive, provide, computed } from 'vue'
import CardList from '../components/CardList.vue'
import { inject } from 'vue'
const items = ref([])
const { cart, addToCart, removeFromCart } = inject('cart')

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearchInput = (event) => {
    filters.searchQuery = event.target.value
  },
  onClickAddPlus = (item) => {
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
    const { data } = await axios.get('https://db22be94fc5d0f63.mokky.dev/items', {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post(`https://db22be94fc5d0f63.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://db22be94fc5d0f63.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log('err')
  }
}
const closeDrawer = () => {
  drawerOpen.value = false
}
onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []
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
