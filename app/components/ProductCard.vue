<script setup>
import { ref } from 'vue'

const props = defineProps({
  product: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['add-to-cart'])

const availableSizes = [38, 39, 40, 41, 42, 43]
const selectedSize = ref(38)

const formatRupiah = (val) => {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR',
    maximumFractionDigits: 0
  }).format(val || 0)
}

const handleAddToCart = () => {
  if (!props.product) return

  const currentStock = props.product.stock?.[selectedSize.value] ?? 1
  if (currentStock <= 0) return

  emit('add-to-cart', {
    id: props.product.id,
    name: props.product.name,
    price: props.product.price,
    image: props.product.image,
    size: selectedSize.value
  })
}
</script>

<template>
  <div v-if="product" class="product-card">
    <div class="thumbnail-wrapper">
      <span v-if="product.badge" class="product-badge">{{ product.badge }}</span>
      <img :src="product.image" :alt="product.name" class="product-img" />
    </div>

    <div class="product-details">
      <h3 class="product-title">{{ product.name }}</h3>
      <p class="product-price">{{ formatRupiah(product.price) }}</p>

      <div class="size-selector">
        <label class="size-label">Pilih Ukuran:</label>
        <div class="size-options">
          <button
            v-for="size in availableSizes"
            :key="size"
            type="button"
            class="btn-size"
            :class="{
              active: selectedSize === size,
              disabled: (product.stock?.[size] ?? 1) <= 0
            }"
            :disabled="(product.stock?.[size] ?? 1) <= 0"
            @click="selectedSize = size"
          >
            {{ size }}
          </button>
        </div>
      </div>

      <button
        type="button"
        class="btn-add-cart"
        :disabled="(product.stock?.[selectedSize] ?? 1) <= 0"
        @click="handleAddToCart"
      >
        {{ (product.stock?.[selectedSize] ?? 1) > 0 ? '+ Tambah ke Keranjang' : 'Stok Ukuran Habis' }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.product-card {
  background: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.2s, box-shadow 0.2s;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px -10px rgba(0, 0, 0, 0.1);
}

.thumbnail-wrapper {
  position: relative;
  width: 100%;
  height: 240px;
  background-color: #f8fafc;
  overflow: hidden;
}

.product-badge {
  position: absolute;
  top: 10px;
  left: 10px;
  background: #0f172a;
  color: #ffffff;
  font-size: 11px;
  font-weight: 700;
  padding: 4px 8px;
  border-radius: 4px;
  z-index: 2;
}

.product-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.product-details {
  padding: 16px;
  display: flex;
  flex-direction: column;
  flex: 1;
}

.product-title {
  font-size: 15px;
  font-weight: 700;
  color: #0f172a;
  margin: 0 0 6px 0;
}

.product-price {
  font-size: 15px;
  font-weight: 800;
  color: #16a34a;
  margin: 0 0 12px 0;
}

.size-selector {
  margin-bottom: 14px;
}

.size-label {
  font-size: 12px;
  color: #64748b;
  display: block;
  margin-bottom: 6px;
}

.size-options {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.btn-size {
  border: 1px solid #cbd5e1;
  background: #ffffff;
  border-radius: 6px;
  padding: 4px 8px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
}

.btn-size.active {
  background: #0f172a;
  color: #ffffff;
  border-color: #0f172a;
}

.btn-size.disabled {
  background: #f1f5f9;
  color: #94a3b8;
  border-color: #e2e8f0;
  text-decoration: line-through;
  cursor: not-allowed;
}

.btn-add-cart {
  margin-top: auto;
  width: 100%;
  padding: 10px;
  background: #0f172a;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  font-size: 13px;
  cursor: pointer;
}

.btn-add-cart:disabled {
  background: #94a3b8;
  cursor: not-allowed;
}
</style>