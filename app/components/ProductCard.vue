<script setup>
defineProps({
  product: {
    type: Object,
    required: true
  }
})

defineEmits(['add-to-cart'])

const formatPrice = (price) => {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR',
    maximumFractionDigits: 0
  }).format(price)
}
</script>

<template>
  <div class="product-card">
    <img :src="product.image" :alt="product.name" class="product-img" />
    <div class="product-info">
      <h3 class="product-title">{{ product.name }}</h3>
      <p class="product-price">{{ formatPrice(product.price) }}</p>

      <div>
        <label class="size-select-label">Pilih Ukuran:</label>
        <select v-model="product.selectedSize">
          <option v-for="size in product.sizes" :key="size" :value="size">
            EU {{ size }}
          </option>
        </select>
      </div>

      <button class="add-cart-btn" @click="$emit('add-to-cart', product)">
        + Tambah ke Keranjang
      </button>
    </div>
  </div>
</template>