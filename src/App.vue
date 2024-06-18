<script setup>
import Header from './components/Header.vue'
import axios from 'axios'
import { ref, watch, inject, provide, computed } from 'vue'
import Drawer from './components/Drawer.vue'

//CART
const cart = ref([])
const isCreatingOrder = ref(false)
const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100)),
  createAnOrder = async () => {
    try {
      isCreatingOrder.value = true
      const { data } = await axios.post('https://db22be94fc5d0f63.mokky.dev/orders', {
        items: cart.value,
        totalPrice: totalPrice.value
      })
      cart.value = []
      return data
    } catch (err) {
      console.log(err)
    } finally {
      isCreatingOrder.value = false
    }
  }
const cartIsEempty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreatingOrder.value || cartIsEempty.value)
const openDrawer = () => {
  drawerOpen.value = true
}
const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}
const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
  console.log(item)
}
const closeDrawer = () => {
  drawerOpen.value = false
}
//CART end
watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', { cart, closeDrawer, openDrawer, addToCart, removeFromCart })
</script>

<template>
  <div class="m-auto w-4/5 bg-white border rounded-xl mt-10">
    <Drawer
      v-if="drawerOpen"
      :total-price="totalPrice"
      :vat-price="vatPrice"
      @create-an-order="createAnOrder"
      :button-disabled="buttonDisabled"
    />
    <Header :total-price="totalPrice" @open-drawer="openDrawer" class="border-b border-slate-300" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
