<script setup>
import { ref, computed } from 'vue'

// Data Produk Sepatu
const products = ref([
  {
    id: 1,
    name: "KAZKA & CO. Vulc Canvas Low - Off White",
    price: 289000,
    image: "https://images.unsplash.com/photo-1525966222134-fcfa99b8ae77?w=600&auto=format&fit=crop&q=60",
    sizes: [38, 39, 40, 41, 42, 43],
    selectedSize: 38
  },
  {
    id: 2,
    name: "KAZKA & CO. Vulc Canvas High - Jet Black",
    price: 319000,
    image: "https://images.unsplash.com/photo-1549298916-b41d501d3772?w=600&auto=format&fit=crop&q=60",
    sizes: [38, 39, 40, 41, 42, 45],
    selectedSize: 38
  },
  {
    id: 3,
    name: "KAZKA & CO. Retro Court - Olive Grey",
    price: 349000,
    image: "https://images.unsplash.com/photo-1560769629-975ec94e6a86?w=600&auto=format&fit=crop&q=60",
    sizes: [40, 41, 42, 43, 44, 45],
    selectedSize: 40
  },
  {
    id: 4,
    name: "KAZKA & CO Vulc Canvas - White and Orange Gum",
    price: 400000,
    image: "/images/gambar-sepatu-1.jpeg",
    sizes: [40, 41, 42, 43, 44, 45],
    selectedSize: 40
  },
  {
    id: 5,
    name: "KAZKA & CO Vulc Canvas - Chocolate and White",
    price: 450000,
    image: "/images/gambar-sepatu-2.jpeg",
    sizes: [40, 41, 42, 43, 44, 45],
    selectedSize: 40
  },
  {
    id: 6,
    name: "KAZKA & CO Vulc - White and Green Gum",
    price: 385000,
    image: "/images/gambar-sepatu-3.jpeg",
    sizes: [40, 41, 42, 43, 44, 45],
    selectedSize: 40
  },
  {
    id: 7,
    name: "KAZKA & CO Vulc Low - White and Cream",
    price: 380000,
    image: "/images/gambar-sepatu-4.jpeg",
    sizes: [40, 41, 42, 43, 44, 45],
    selectedSize: 40
  },
  {
    id: 8,
    name: "KAZKA & CO Vulc Low - Milk",
    price: 390000,
    image: "/images/gambar-sepatu-5.jpeg",
    sizes: [40, 41, 42, 43, 44, 45],
    selectedSize: 40
  },
  {
    id: 9,
    name: "KAZKA & CO Vulc Low - Chocolate Gum",
    price: 395000,
    image: "/images/gambar-sepatu-6.jpeg",
    sizes: [40, 41, 42, 43, 44, 45],
    selectedSize: 40
  }
])

// State Keranjang & Pop-up
const cart = ref([])
const isCartOpen = ref(false)
const isSizeChartOpen = ref(false)

// Form Pembeli
const customerName = ref('')
const customerEmail = ref('')
const customerPhone = ref('')

// Perhitungan Otomatis
const cartCount = computed(() => cart.value.reduce((sum, item) => sum + item.qty, 0))
const cartTotal = computed(() => cart.value.reduce((sum, item) => sum + (item.price * item.qty), 0))

function formatRupiah(num) {
  return new Intl.NumberFormat("id-ID", { style: "currency", currency: "IDR", maximumFractionDigits: 0 }).format(num)
}

function addToCart(product) {
  const existingItem = cart.value.find(item => item.id === product.id && item.size === product.selectedSize)
  if (existingItem) {
    existingItem.qty += 1
  } else {
    cart.value.push({
      id: product.id,
      name: product.name,
      price: product.price,
      size: product.selectedSize,
      qty: 1
    })
  }
  isCartOpen.value = true
}

function changeQty(productId, size, delta) {
  const idx = cart.value.findIndex(item => item.id === productId && item.size === size)
  if (idx > -1) {
    cart.value[idx].qty += delta
    if (cart.value[idx].qty <= 0) {
      cart.value.splice(idx, 1)
    }
  }
}

function handleCheckout() {
  if (cart.value.length === 0) {
    alert("Keranjang masih kosong!")
    return
  }
  if (!customerName.value.trim()) {
    alert("Mohon isi Nama Lengkap pemesan.")
    return
  }
  alert(`Pesanan atas nama ${customerName.value} berhasil disiapkan!`)
}
</script>

<template>
  <div>
    <!-- Navbar -->
    <Navbar :cart-count="cartCount" @open-cart="isCartOpen = true" />

    <!-- Hero Section -->
    <section class="hero">
      <h1>Sepatu Lokal Kualitas Esensial</h1>
      <p>Dibuat dengan kanvas pilihan dan sol vulkanisir untuk kenyamanan harian.</p>
    </section>

    <!-- Katalog Produk -->
    <main class="catalog-container">
      <div class="catalog-header">
        <h2>Koleksi Artikel</h2>
        <button type="button" class="size-chart-trigger" @click="isSizeChartOpen = true">
          📏 Panduan Ukuran (Size Chart)
        </button>
      </div>

      <div class="products-grid">
        <ProductCard
          v-for="product in products"
          :key="product.id"
          :product="product"
          @add-to-cart="addToCart"
        />
      </div>
    </main>

    <!-- Modal Pop-up Size Chart -->
    <div class="modal" :class="{ open: isSizeChartOpen }">
      <div class="modal-dialog">
        <div class="modal-header">
          <h3>Panduan Ukuran Sepatu (EU Sizing)</h3>
          <button class="close-btn" @click="isSizeChartOpen = false">&times;</button>
        </div>
        <div class="modal-body">
          <p class="modal-desc">
            Ukur panjang telapak kaki Anda dari ujung tumit hingga ujung jari terpanjang menggunakan penggaris dalam satuan centimeter (cm).
          </p>
          <table class="size-table">
            <thead>
              <tr>
                <th>Ukuran (EU)</th>
                <th>Panjang Kaki (cm)</th>
                <th>Insole (cm)</th>
              </tr>
            </thead>
            <tbody>
              <tr><td>38</td><td>24.0 cm</td><td>24.7 cm</td></tr>
              <tr><td>39</td><td>24.5 cm</td><td>25.3 cm</td></tr>
              <tr><td>40</td><td>25.5 cm</td><td>26.0 cm</td></tr>
              <tr><td>41</td><td>26.0 cm</td><td>26.6 cm</td></tr>
              <tr><td>42</td><td>27.0 cm</td><td>27.3 cm</td></tr>
              <tr><td>43</td><td>27.5 cm</td><td>28.0 cm</td></tr>
              <tr><td>44</td><td>28.5 cm</td><td>28.7 cm</td></tr>
              <tr><td>45</td><td>29.0 cm</td><td>29.5 cm</td></tr>
            </tbody>
          </table>
          <div class="size-tip">
            <strong>Tips:</strong> Jika tipe kaki Anda lebar (*wide foot*), disarankan memilih 1 nomor di atas ukuran standar (*up-size*).
          </div>
        </div>
      </div>
    </div>

    <!-- Drawer Keranjang Belanja -->
    <aside class="cart-drawer" :class="{ open: isCartOpen }">
      <div class="cart-header">
        <h3>Keranjang Belanja</h3>
        <button class="close-btn" @click="isCartOpen = false">&times;</button>
      </div>

      <div class="cart-items">
        <p v-if="cart.length === 0" class="empty-msg">Keranjang masih kosong.</p>
        <div v-for="item in cart" :key="item.id + '-' + item.size" class="cart-item">
          <div class="cart-item-detail">
            <strong>{{ item.name }}</strong>
            <span class="cart-item-meta">Ukuran: EU {{ item.size }} | {{ formatRupiah(item.price) }}</span>
          </div>
          <div class="cart-qty-ctrl">
            <button type="button" @click="changeQty(item.id, item.size, -1)">-</button>
            <span>{{ item.qty }}</span>
            <button type="button" @click="changeQty(item.id, item.size, 1)">+</button>
          </div>
        </div>
      </div>

      <div class="cart-footer">
        <div class="customer-form">
          <label class="form-label">Nama Lengkap</label>
          <input v-model="customerName" type="text" class="form-input" placeholder="Contoh: Azka Kurnia">

          <label class="form-label">Email</label>
          <input v-model="customerEmail" type="email" class="form-input" placeholder="Contoh: nama@email.com">

          <label class="form-label">Nomor WhatsApp</label>
          <input v-model="customerPhone" type="tel" class="form-input" placeholder="Contoh: 08123456789">
        </div>

        <div class="total-row">
          <span>Total:</span>
          <strong>{{ formatRupiah(cartTotal) }}</strong>
        </div>
        <button class="checkout-btn" @click="handleCheckout">Lanjut ke Pembayaran</button>
      </div>
    </aside>

    <!-- Overlay Background -->
    <div 
      class="overlay" 
      :class="{ open: isCartOpen || isSizeChartOpen }" 
      @click="isCartOpen = false; isSizeChartOpen = false"
    ></div>
  </div>
</template>
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

body {
    background-color: #fcfcfc;
    color: #1a1a1a;
    min-height: 100vh;
}

/* Navbar */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 32px;
    background: #ffffff;
    border-bottom: 1px solid #eaeaea;
    position: sticky;
    top: 0;
    z-index: 10;
}

.brand-logo {
    font-size: 22px;
    font-weight: 800;
    letter-spacing: 1.5px;
}

.cart-btn {
    background: #111;
    color: #fff;
    border: none;
    padding: 8px 16px;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    font-size: 14px;
}

/* Hero Section */
.hero {
    text-align: center;
    padding: 48px 20px 32px;
    background-color: #f7f7f8;
    border-bottom: 1px solid #ececec;
}

.hero h1 {
    font-size: 28px;
    margin-bottom: 8px;
}

.hero p {
    color: #666;
    font-size: 15px;
}

/* Katalog */
.catalog-container {
    max-width: 1080px;
    margin: 32px auto;
    padding: 0 20px;
}

.catalog-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 24px;
}

.catalog-header h2 {
    font-size: 20px;
}

.size-chart-trigger {
    background: none;
    border: 1px dashed #666;
    padding: 6px 12px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 13px;
    font-weight: 600;
    color: #333;
}

.size-chart-trigger:hover {
    background: #f0f0f0;
}

/* Grid & Card Produk */
.products-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 24px;
}

.product-card {
    background: #fff;
    border: 1px solid #eee;
    border-radius: 8px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
}

.product-img {
    width: 100%;
    height: 220px;
    object-fit: cover;
    background-color: #eee;
}

.product-info {
    padding: 16px;
    display: flex;
    flex-direction: column;
    flex: 1;
}

.product-title {
    font-size: 15px;
    margin-bottom: 6px;
}

.product-price {
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 12px;
    color: #222;
}

.size-select-label {
    font-size: 12px;
    color: #666;
    margin-bottom: 4px;
}

.size-dropdown {
    padding: 6px 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-bottom: 14px;
    font-size: 13px;
}

.add-cart-btn {
    margin-top: auto;
    background: #111;
    color: #fff;
    border: none;
    padding: 10px;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    font-size: 13px;
}

.add-cart-btn:hover {
    background: #333;
}

/* Modal Pop-up */
.modal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 40;
    align-items: center;
    justify-content: center;
    padding: 20px;
}

.modal.open {
    display: flex;
}

.modal-dialog {
    background: #ffffff;
    width: 100%;
    max-width: 480px;
    border-radius: 8px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
    overflow: hidden;
    animation: modalFadeIn 0.2s ease-out;
}

@keyframes modalFadeIn {
    from {
        transform: translateY(-15px);
        opacity: 0;
    }

    to {
        transform: translateY(0);
        opacity: 1;
    }
}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 20px;
    border-bottom: 1px solid #eee;
}

.modal-header h3 {
    font-size: 16px;
    font-weight: 700;
}

.close-btn {
    background: none;
    border: none;
    font-size: 22px;
    cursor: pointer;
    line-height: 1;
}

.modal-body {
    padding: 20px;
}

.modal-desc {
    font-size: 13px;
    color: #666;
    margin-bottom: 16px;
    line-height: 1.5;
}

.size-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
    margin-bottom: 16px;
}

.size-table th,
.size-table td {
    border: 1px solid #eee;
    padding: 8px 12px;
    text-align: center;
}

.size-table th {
    background-color: #f7f7f8;
    font-weight: 600;
    color: #333;
}

.size-tip {
    background-color: #fdf8e2;
    border-left: 3px solid #e0b028;
    padding: 10px 12px;
    font-size: 12px;
    color: #554300;
    border-radius: 4px;
}

/* Drawer Keranjang */
.cart-drawer {
    position: fixed;
    top: 0;
    right: -420px;
    width: 100%;
    max-width: 400px;
    height: 100vh;
    background: #ffffff;
    box-shadow: -4px 0 15px rgba(0, 0, 0, 0.1);
    z-index: 30;
    display: flex;
    flex-direction: column;
    transition: right 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.cart-drawer.open {
    right: 0;
}

.cart-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 20px;
    border-bottom: 1px solid #eee;
}

.cart-items {
    flex: 1;
    overflow-y: auto;
    padding: 20px;
}

.empty-msg {
    text-align: center;
    color: #888;
    margin-top: 40px;
    font-size: 14px;
}

.cart-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 12px;
    margin-bottom: 12px;
    border-bottom: 1px solid #f0f0f0;
}

.cart-item-detail {
    display: flex;
    flex-direction: column;
    gap: 2px;
    font-size: 13px;
}

.cart-item-meta {
    color: #666;
    font-size: 12px;
}

.cart-qty-ctrl {
    display: flex;
    align-items: center;
    gap: 8px;
}

.cart-qty-ctrl button {
    width: 24px;
    height: 24px;
    border: 1px solid #ccc;
    background: #fff;
    border-radius: 4px;
    cursor: pointer;
}

/* Footer & Form Checkout */
.cart-footer {
    padding: 20px;
    border-top: 1px solid #eee;
    background: #fafafa;
}

.customer-form {
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-bottom: 14px;
    padding-bottom: 12px;
    border-bottom: 1px dashed #e0e0e0;
}

.form-label {
    font-size: 12px;
    font-weight: 600;
    color: #444;
}

.form-input {
    width: 100%;
    padding: 8px 10px;
    font-size: 13px;
    border: 1px solid #ccc;
    border-radius: 5px;
    outline: none;
}

.form-input:focus {
    border-color: #111;
}

.form-input.error {
    border-color: #dc2626;
    background-color: #fef2f2;
}

.total-row {
    display: flex;
    justify-content: space-between;
    font-size: 16px;
    margin-bottom: 14px;
}

.checkout-btn {
    width: 100%;
    background: #16a34a;
    color: #fff;
    border: none;
    padding: 12px;
    border-radius: 6px;
    font-weight: 700;
    font-size: 14px;
    cursor: pointer;
}

.checkout-btn:hover {
    background: #15803d;
}

/* Overlay */
.overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.45);
    z-index: 20;
}

.overlay.open {
    display: block;
}
</style>