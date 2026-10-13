<script setup>
defineProps({
  cartCount: {
    type: Number,
    default: 0
  }
})

defineEmits(['open-size-chart', 'open-cart'])
</script>

<template>
  <header class="navbar">
    <div class="navbar-container">
      <div class="brand">
        <span class="brand-title">KAZKA &amp; CO.</span>
      </div>

      <div class="nav-actions">
        <button class="nav-btn-link" @click="$emit('open-size-chart')">
          Panduan Ukuran
        </button>

        <button class="cart-btn" @click="$emit('open-cart')">
          🛒 Keranjang
          <span v-if="cartCount > 0" class="cart-badge">{{ cartCount }}</span>
        </button>
      </div>
    </div>
  </header>
</template>

<style scoped>
.navbar {
  background-color: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  position: sticky;
  top: 0;
  z-index: 50;
}

.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 14px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.brand-title {
  font-size: 18px;
  font-weight: 800;
  letter-spacing: 1px;
  color: #0f172a;
}

.nav-actions {
  display: flex;
  align-items: center;
  gap: 16px;
}

.nav-btn-link {
  background: none;
  border: none;
  font-size: 13px;
  font-weight: 500;
  color: #64748b;
  cursor: pointer;
  transition: color 0.2s;
}

.nav-btn-link:hover {
  color: #0f172a;
}

.cart-btn {
  position: relative;
  background-color: #0f172a;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  padding: 8px 16px;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
}

.cart-badge {
  background-color: #ef4444;
  color: #ffffff;
  font-size: 11px;
  font-weight: 700;
  padding: 2px 6px;
  border-radius: 10px;
}
</style>