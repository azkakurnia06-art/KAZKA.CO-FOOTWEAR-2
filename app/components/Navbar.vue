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
  <header class="navbar-wrapper">
    <div class="navbar-container">
      <!-- 1. Logo Kiri -->
      <div class="navbar-logo">
        <a href="/" class="brand-title">KAZKA & CO.</a>
      </div>

      <!-- 2. Menu Navigasi Tengah -->
      <nav class="navbar-menu">
        <a href="#" class="nav-link active">Home</a>
        <a href="#katalog" class="nav-link">Products</a>
        <a href="#" class="nav-link">Sports</a>
        <a href="#" class="nav-link">Contact</a>
      </nav>

      <!-- 3. Aksi Kanan (Panduan Ukuran, Search, Profil, Keranjang) -->
      <div class="navbar-actions">
        <button class="btn-guide" @click="$emit('open-size-chart')">
          Panduan Ukuran
        </button>

        <!-- Tombol Search -->
        <button class="icon-btn" title="Pencarian" aria-label="Search">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="11" cy="11" r="8"></circle>
            <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
          </svg>
        </button>

        <!-- Tombol Akun / User -->
        <button class="icon-btn" title="Akun" aria-label="User Account">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
          </svg>
        </button>

        <!-- Tombol Keranjang -->
        <button class="btn-cart" @click="$emit('open-cart')">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M6 2L3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4z"></path>
            <line x1="3" y1="6" x2="21" y2="6"></line>
            <path d="M16 10a4 4 0 0 1-8 0"></path>
          </svg>
          <span class="cart-text">Keranjang</span>
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

.navbar-wrapper {
  background: #ffffff;
  border-bottom: 1px solid #f1f5f9;
  position: sticky;
  top: 0;
  z-index: 100;
  width: 100%;
}

.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 16px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
}

.brand-title {
  font-size: 18px;
  font-weight: 900;
  letter-spacing: 1.5px;
  color: #0f172a;
  text-decoration: none;
}

/* Navigasi Tengah */
.navbar-menu {
  display: flex;
  align-items: center;
  gap: 28px;
}

.nav-link {
  font-size: 14px;
  font-weight: 500;
  color: #334155;
  text-decoration: none;
  transition: color 0.2s ease;
}

.nav-link:hover,
.nav-link.active {
  color: #064e3b;
  font-weight: 700;
}

/* Bagian Kanan */
.navbar-actions {
  display: flex;
  align-items: center;
  gap: 16px;
}

.btn-guide {
  background: transparent;
  border: none;
  font-size: 13px;
  font-weight: 600;
  color: #64748b;
  cursor: pointer;
  padding: 6px 10px;
  transition: color 0.2s ease;
}

.btn-guide:hover {
  color: #0f172a;
}

.icon-btn {
  background: transparent;
  border: none;
  color: #334155;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 6px;
  border-radius: 50%;
  transition: background-color 0.2s ease, color 0.2s ease;
}

.icon-btn:hover {
  background-color: #f1f5f9;
  color: #0f172a;
}

.btn-cart {
  background: #0f172a;
  color: #ffffff;
  border: none;
  padding: 8px 18px;
  border-radius: 8px;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: opacity 0.2s ease;
}

.btn-cart:hover {
  opacity: 0.9;
}

.cart-badge {
  background: #10b981;
  color: #ffffff;
  font-size: 11px;
  font-weight: 700;
  padding: 2px 6px;
  border-radius: 10px;
}

@media (max-width: 860px) {
  .navbar-menu {
    display: none;
  }
}
</style>