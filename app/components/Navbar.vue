<script setup>
defineProps({
  cartCount: {
    type: Number,
    default: 0
  }
})

defineEmits(['open-cart'])
</script>

<template>
  <header class="navbar">
    <div class="brand-logo">KAZKA & CO.</div>
    <button class="cart-btn" @click="$emit('open-cart')">
      Keranjang (<span id="cartCount">{{ cartCount }}</span>)
    </button>
  </header>
</template>