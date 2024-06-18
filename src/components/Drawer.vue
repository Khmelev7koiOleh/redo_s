<script setup>
import DrawerHead from '../components/DrawerHead.vue'
import CartItemList from '../components/CartItemList.vue'
import infoBlock from './infoBlock.vue'
import { inject } from 'vue'
const { closeDrawer } = inject('cart')

const emit = defineEmits(['CreateAnOrder'])
defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Function
})
</script>

<template>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <div>
      <DrawerHead @click="closeDrawer" />
    </div>
    <CartItemList />
    <div>
      <div v-if="!totalPrice" class="flex flex-col h-screen justify-center">
        <infoBlock
          title="The cart is empty"
          description="add at least one pair of sneakers to make an order "
          imageUrl="/package-icon.png"
          buttonBack="Get back"
        />
      </div>
      <div v-if="totalPrice">
        <div class="flex items-center">
          <b>In Total:</b>
          <div class="flex-1 border border-dashed"></div>
          <div class="font-bold">{{ totalPrice }}</div>
        </div>

        <div class="flex items-center">
          <b>Taxes 5%</b>
          <div class="flex-1 border border-dashed"></div>
          <div class="font-bold">{{ vatPrice }}</div>
        </div>
        <div @click="() => emit('CreateAnOrder')">
          <button
            :disabled="buttonDisabled"
            class="mt-8 bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-400 hover:bg-lime-600 active:bg-lime-700 transition cursor-pointer"
          >
            Place an order
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
