<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import Navbar from '~/components/Navbar.vue'
import ProductCard from '~/components/ProductCard.vue'
import SizeChartModal from '~/components/SizeChartModal.vue'
import CartDrawer from '~/components/CartDrawer.vue'
import InvoiceModal from '~/components/InvoiceModal.vue' // <-- TARUH DI SINI

// 1. Data Katalog Produk (Lengkap dengan properti category)
const products = ref([
  {
    id: 1,
    name: 'KAZKA & CO. Vulc Canvas Low - Off White',
    price: 289000,
    category: 'low',
    badge: 'Best Seller',
    image: '/images/gambar-sepatu-1.jpeg',
    stock: { 38: 5, 39: 0, 40: 8, 41: 3, 42: 0, 43: 4 }
  },
  {
    id: 2,
    name: 'KAZKA & CO. Vulc Canvas High - Jet Black',
    price: 319000,
    category: 'high',
    badge: 'New Arrival',
    image: '/images/gambar-sepatu-2.jpeg',
    stock: { 38: 2, 39: 4, 40: 0, 41: 5, 42: 2, 43: 1 }
  },
  {
    id: 3,
    name: 'KAZKA & CO. Retro Court - Olive Grey',
    price: 349000,
    category: 'court',
    badge: 'Diskon 10%',
    image: '/images/gambar-sepatu-3.jpeg',
    stock: { 38: 0, 39: 3, 40: 5, 41: 0, 42: 6, 43: 2 }
  },
  {
    id: 4,
    name: 'KAZKA & CO. Retro Court - Cream Gum',
    price: 349000,
    category: 'court',
    badge: null,
    image: '/images/gambar-sepatu-4.jpeg',
    stock: { 38: 4, 39: 2, 40: 3, 41: 2, 42: 5, 43: 0 }
  },
  {
    id: 5,
    name: 'KAZKA & CO. Slip-On Classic - Checkerboard',
    price: 269000,
    category: 'slip on',
    badge: 'Hot Item',
    image: '/images/gambar-sepatu-5.jpeg',
    stock: { 38: 3, 39: 0, 40: 2, 41: 4, 42: 0, 43: 3 }
  },
  {
    id: 6,
    name: 'KAZKA & CO. Slip-On Lite - Triple Black',
    price: 259000,
    category: 'slip on',
    badge: null,
    image: '/images/gambar-sepatu-6.jpeg',
    stock: { 38: 5, 39: 4, 40: 6, 41: 3, 42: 2, 43: 1 }
  },
  {
    id: 7,
    name: 'KAZKA & CO. Vulc Canvas Low - Off White',
    price: 289000,
    category: 'low',
    badge: 'Best Seller',
    image: '/images/gambar-sepatu-7.jpeg',
    stock: { 38: 5, 39: 0, 40: 8, 41: 3, 42: 0, 43: 4 }
  },
  {
    id: 8,
    name: 'KAZKA & CO. Vulc Canvas High - Jet Black',
    price: 319000,
    category: 'high',
    badge: 'New Arrival',
    image: '/images/gambar-sepatu-8.jpeg',
    stock: { 38: 2, 39: 4, 40: 0, 41: 5, 42: 2, 43: 1 }
  },
  {
    id: 9,
    name: 'KAZKA & CO. Retro Court - Olive Grey',
    price: 349000,
    category: 'court',
    badge: 'Diskon 10%',
    image: '/images/gambar-sepatu-9.jpeg',
    stock: { 38: 0, 39: 3, 40: 5, 41: 0, 42: 6, 43: 2 }
  },
  {
    id: 10,
    name: 'KAZKA & CO. Retro Court - Cream Gum',
    price: 349000,
    category: 'court',
    badge: null,
    image: '/images/gambar-sepatu-10.jpeg',
    stock: { 38: 4, 39: 2, 40: 3, 41: 2, 42: 5, 43: 0 }
  },
  {
    id: 11,
    name: 'KAZKA & CO. Slip-On Classic - Checkerboard',
    price: 269000,
    category: 'slip on',
    badge: 'Hot Item',
    image: '/images/gambar-sepatu-11.jpeg',
    stock: { 38: 3, 39: 0, 40: 2, 41: 4, 42: 0, 43: 3 }
  },
  {
    id: 12,
    name: 'KAZKA & CO. Slip-On Lite - Triple Black',
    price: 259000,
    category: 'slip on',
    badge: null,
    image: '/images/gambar-sepatu-12.jpeg',
    stock: { 38: 5, 39: 4, 40: 6, 41: 3, 42: 2, 43: 1 }
  },
  {
    id: 13,
    name: 'KAZKA & CO. Women Court - Pastel Blossom',
    price: 290000,
    category: 'court',
    badge: 'Hot Item',
    image: '/images/gambar-sepatu-cewek.jpeg',
    stock: { 38: 3, 39: 0, 40: 2, 41: 4, 42: 0, 43: 3 }
  },
  {
    id: 14,
    name: 'KAZKA & CO. Baby Series - Minimal White',
    price: 260000,
    category: 'low',
    badge: 'Hot Item',
    image: '/images/gambar-sepatu-baby.jpeg',
    stock: { 38: 3, 39: 0, 40: 2, 41: 4, 42: 0, 43: 3 }
  },
  {
    id: 15,
    name: 'KAZKA & CO. Kids Urban - Mono Stripe',
    price: 270000,
    category: 'low',
    badge: 'Hot Item',
    image: '/images/gambar-sepatu-kids.jpeg',
    stock: { 38: 3, 39: 0, 40: 2, 41: 4, 42: 0, 43: 3 }
  },
  {
    id: 16,
    name: 'KAZKA & CO. Men Classic - Sport Striped',
    price: 300000,
    category: 'court',
    badge: 'Hot Item',
    image: '/images/gambar-sepatu-cowok.jpeg',
    stock: { 38: 3, 39: 0, 40: 2, 41: 4, 42: 0, 43: 3 }
  }
]);

// 2. Search & Category Filter State
const searchQuery = ref('')
const selectedCategory = ref('all')

const sortBy = ref('default') // default, price-asc, price-desc, name-asc

const filteredProducts = computed(() => {
  if (!products.value || !Array.isArray(products.value)) return []

  // 1. Filter Kategori & Pencarian dengan perlindungan aman
  let list = products.value.filter((item) => {
    if (!item) return false
    
    const matchCategory = 
      selectedCategory.value === 'all' || 
      (item.category && item.category.toLowerCase() === selectedCategory.value.toLowerCase())

    const query = searchQuery.value ? searchQuery.value.toLowerCase().trim() : ''
    const matchSearch = item.name ? item.name.toLowerCase().includes(query) : true

    return matchCategory && matchSearch
  })

  // 2. Pengurutan / Sorting
  if (sortBy.value === 'price-asc') {
    list.sort((a, b) => (a.price || 0) - (b.price || 0))
  } else if (sortBy.value === 'price-desc') {
    list.sort((a, b) => (b.price || 0) - (a.price || 0))
  } else if (sortBy.value === 'name-asc') {
    list.sort((a, b) => (a.name || '').localeCompare(b.name || ''))
  }

  return list
})

// 3. UI State
const isCartOpen = ref(false)
const isSizeChartOpen = ref(false)
const isInvoiceOpen = ref(false) // <-- BARU
const currentInvoice = ref(null) // <-- BARU



// 4. Cart State & LocalStorage Persistence
const cart = ref([])

onMounted(() => {
  const savedCart = localStorage.getItem('kazka_cart')
  if (savedCart) {
    try {
      cart.value = JSON.parse(savedCart)
    } catch (e) {
      cart.value = []
    }
  }
})

watch(cart, (newCart) => {
  localStorage.setItem('kazka_cart', JSON.stringify(newCart))
}, { deep: true })

const cartCount = computed(() => {
  return cart.value.reduce((total, item) => total + item.qty, 0)
})

// Hitung total belanja secara aman
const cartTotal = computed(() => {
  if (!cart.value || !Array.isArray(cart.value)) return 0
  return cart.value.reduce((total, item) => {
    if (!item || !item.price || !item.qty) return total
    return total + (item.price * item.qty)
  }, 0)
})

// 5. Toast Notification
const toastMessage = ref('')
const showToast = ref(false)
let toastTimeout = null

const triggerToast = (msg) => {
  toastMessage.value = msg
  showToast.value = true
  if (toastTimeout) clearTimeout(toastTimeout)
  toastTimeout = setTimeout(() => {
    showToast.value = false
  }, 2500)
}

// 6. Cart Actions
// Tambah ke keranjang secara aman
const addToCart = (productData) => {
  if (!productData || !productData.id) return

  if (!cart.value) cart.value = []

  const existingItem = cart.value.find(
    (item) => item && item.id === productData.id && item.size === productData.size
  )

  if (existingItem) {
    existingItem.qty = (existingItem.qty || 1) + 1
  } else {
    cart.value.push({
      ...productData,
      qty: 1
    })
  }

  showToast.value = true
  toastMessage.value = `${productData.name} (Size ${productData.size}) berhasil ditambahkan!`
  setTimeout(() => {
    showToast.value = false
  }, 2500)
}

const changeQty = ({ id, size, delta }) => {
  const itemIndex = cart.value.findIndex(
    (item) => item.id === id && item.size === size
  )
  if (itemIndex > -1) {
    cart.value[itemIndex].qty += delta
    if (cart.value[itemIndex].qty <= 0) {
      cart.value.splice(itemIndex, 1)
    }
  }
}

const handleCheckout = (orderData) => {
  const now = new Date()
  const invoiceId = 'KZK-' + Math.floor(100000 + Math.random() * 900000)
  const dateFormatted = now.toLocaleDateString('id-ID', {
    day: 'numeric',
    month: 'short',
    year: 'numeric'
  })

  currentInvoice.value = {
    ...orderData,
    invoiceId,
    date: dateFormatted,
    items: [...cart.value],
    subtotal: cartTotal.value
  }

  cart.value = []
  if (import.meta.client) {
    localStorage.removeItem('cart')
  }

  isCartOpen.value = false
  isInvoiceOpen.value = true
}

// 2. Tambahkan fungsi ini di bawah handleCheckout:
const sendInvoiceToWhatsApp = (invoice) => {
  let itemListText = ''
  invoice.items.forEach((item, index) => {
    itemListText += `${index + 1}. *${item.name}*\n`
    itemListText += `   • Ukuran: EU ${item.size}\n`
    itemListText += `   • Qty: ${item.qty}\n`
    itemListText += `   • Subtotal: Rp ${(item.price * item.qty).toLocaleString('id-ID')}\n\n`
  })

  const ADMIN_WHATSAPP = '6281234567890'

  const message = 
`*PESANAN RESMI: ${invoice.orderId}*
KAZKA & CO. FOOTWEAR

*Detail Pembeli:*
• Nama: ${invoice.name}
• No. WhatsApp: ${invoice.phone}
${invoice.email ? `• Email: ${invoice.email}\n` : ''}
*Daftar Artikel:*
${itemListText.trim()}

*Pengiriman:*
• Kurir: ${invoice.courier}
• Ongkir: Rp ${invoice.shippingCost.toLocaleString('id-ID')}

*TOTAL PEMBAYARAN:* *Rp ${invoice.totalPayment.toLocaleString('id-ID')}*

Halo Admin, saya sudah menerima invoice di web dan ingin konfirmasi pembayaran.`

  const encodedMessage = encodeURIComponent(message)
  window.open(`https://wa.me/${ADMIN_WHATSAPP}?text=${encodedMessage}`, '_blank')

  // Bersihkan keranjang dan tutup modal
  cart.value = []
  isInvoiceOpen.value = false
  triggerToast('Pesanan diteruskan ke WhatsApp Admin!')
}
</script>

<template>
  <div class="store-layout">
    <!-- Navbar Component -->
    <Navbar
      :cart-count="cartCount"
      @open-size-chart="isSizeChartOpen = true"
      @open-cart="isCartOpen = true"
    />

    <!-- WRAPPER UTAMA (Mengunci lebar banner hijau & mosaik agar sejajar) -->
    <div class="hero-mosaic-wrapper">

      <!-- 1. Hero Section (Banner Hijau Atas) -->
      <!-- 1. Hero Section -->
      <section class="hero-section">
        <div class="hero-container">
          <div class="hero-content">
            <span class="hero-badge">NEW COLLECTION 2026</span>
            <h1 class="hero-title">WEEKEND SALE</h1>
            <p class="hero-subtitle">-20% CUT FOR ALL RUNNING FOOTWEAR</p>
            <a href="#katalog" class="btn-hero">Explore Collection →</a>
          </div>
          
          <div class="hero-image-wrapper">
            <img src="/images/gambar-sepau-tanpa-bg-1-removebg-preview.png" alt="Hero Shoes" class="hero-shoe" />
          </div>
        </div>
      </section>

      <!-- 2. Grid Mosaik Kategori (Bawah Banner) -->
      <section class="category-mosaic-section">
        <!-- Kolom Kiri: WOMEN -->
        <div class="mosaic-item">
          <img src="/images/gambar-sepatu-cewek.jpeg" alt="Women Collection" />
          <div class="mosaic-overlay">
            <h3 class="mosaic-title">WOMEN</h3>
            <span class="mosaic-sub">NEW COLLECTION</span>
          </div>
        </div>

        <!-- Kolom Tengah: BABYS & KIDS -->
        <div class="mosaic-center-stack">
          <div class="mosaic-item">
            <img src="/images/gambar-sepatu-baby.jpeg" alt="Babys Collection" />
            <div class="mosaic-overlay">
              <h3 class="mosaic-title">BABYS</h3>
              <span class="mosaic-sub">NEW COLLECTION</span>
            </div>
          </div>

          <div class="mosaic-item">
            <img src="/images/gambar-sepatu-kids.jpeg" alt="Kids & Young" />
            <div class="mosaic-overlay">
              <h3 class="mosaic-title">KIDS & YOUNG</h3>
              <span class="mosaic-sub">NEW COLLECTION</span>
            </div>
          </div>
        </div>

        <!-- Kolom Kanan: MEN -->
        <div class="mosaic-item">
          <img src="/images/gambar-sepatu-cowok.jpeg" alt="Men Collection" />
          <div class="mosaic-overlay">
            <h3 class="mosaic-title">MEN</h3>
            <span class="mosaic-sub">NEW COLLECTION</span>
          </div>
        </div>
      </section>

    </div>

    <!-- Main Content -->
    <main class="main-content" id="katalog">
      <header class="catalog-header">
        <h1 class="catalog-title">Katalog Artikel</h1>
        <p class="catalog-subtitle">Pilihan siluet esensial KAZKA & CO. untuk aktivitas harian.</p>
      </header>

      <!-- Toolbar Filter & Search -->
      <section class="catalog-toolbar">
        <div class="search-box">
          <input 
            v-model="searchQuery" 
            type="text" 
            placeholder="Cari artikel sepatu..." 
            class="search-input"
          />
        </div>

        <!-- Dropdown Urutkan / Sorting -->
        <div class="filter-controls">
          <select v-model="sortBy" class="sort-select">
            <option value="default">Urutkan: Rekomendasi</option>
            <option value="price-asc">Harga: Termurah</option>
            <option value="price-desc">Harga: Tertinggi</option>
            <option value="name-asc">Nama: A - Z</option>
          </select>
        </div>

        <div class="category-tabs">
          <button 
            :class="{ active: selectedCategory === 'all' }" 
            @click="selectedCategory = 'all'"
          >Semua</button>
          <button 
            :class="{ active: selectedCategory === 'low' }" 
            @click="selectedCategory = 'low'"
          >Low Top</button>
          <button 
            :class="{ active: selectedCategory === 'high' }" 
            @click="selectedCategory = 'high'"
          >High Top</button>
          <button 
            :class="{ active: selectedCategory === 'court' }" 
            @click="selectedCategory = 'court'"
          >Court</button>
          <button 
            :class="{ active: selectedCategory === 'slip on' }" 
            @click="selectedCategory = 'slip on'"
          >Slip On</button>
        </div>
      </section>

      <!-- Grid Produk Terfilter -->
      <section class="products-grid">
        <ProductCard
          v-for="item in filteredProducts"
          :key="item.id"
          :product="item"
          @add-to-cart="addToCart"
        />
      </section>

      <p v-if="filteredProducts.length === 0" class="no-products">
        Artikel sepatu tidak ditemukan.
      </p>
    </main>

    <!-- Modal Panduan Ukuran -->
    <SizeChartModal 
      :is-open="isSizeChartOpen" 
      @close="isSizeChartOpen = false" 
    />

    <!-- Drawer Keranjang Belanja -->
    <CartDrawer
      :is-open="isCartOpen"
      :cart="cart"
      :cart-total="cartTotal"
      @close="isCartOpen = false"
      @change-qty="changeQty"
      @checkout="handleCheckout"
    />

    <!-- Modal Invoice Bukti Pemesanan -->
    <InvoiceModal
      :is-open="isInvoiceOpen"
      :invoice-data="currentInvoice"
      @close="isInvoiceOpen = false"
      @confirm-whatsapp="sendInvoiceToWhatsApp"
    />

    <!-- Toast Notification Melayang -->
    <div class="toast" :class="{ show: showToast }">
      ✓ {{ toastMessage }}
    </div>
  </div>
</template>

<style scoped>
.store-layout {
  min-height: 100vh;
  background-color: #fafafa;
  color: #1e293b;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}

.main-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px 80px;
}

.catalog-header {
  margin-bottom: 28px;
}

.catalog-title {
  font-size: 26px;
  font-weight: 700;
  letter-spacing: -0.5px;
  margin-bottom: 6px;
}

.catalog-subtitle {
  color: #64748b;
  font-size: 15px;
}

/* Toolbar: Search & Tabs */
.catalog-toolbar {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 32px;
}

@media (min-width: 640px) {
  .catalog-toolbar {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}

.search-input {
  width: 100%;
  max-width: 320px;
  padding: 10px 16px;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  font-size: 14px;
  background-color: #ffffff;
  outline: none;
  transition: border-color 0.2s;
}

.search-input:focus {
  border-color: #0f172a;
}

.category-tabs {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-bottom: 24px;
}

.category-tabs button {
  padding: 8px 18px;
  border-radius: 9999px;
  border: 1px solid #e2e8f0;
  background-color: #ffffff;
  color: #475569;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.category-tabs button:hover {
  background-color: #f8fafc;
  border-color: #cbd5e1;
}

.category-tabs button.active {
  background-color: #0f172a;
  border-color: #0f172a;
  color: #ffffff;
}

/* Grid Katalog */
.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 24px;
}

.no-products {
  text-align: center;
  color: #64748b;
  margin: 60px 0;
  font-size: 15px;
}

/* Toast Floating */
.toast {
  position: fixed;
  bottom: 24px;
  right: 24px;
  background-color: #0f172a;
  color: #ffffff;
  padding: 12px 20px;
  border-radius: 8px;
  font-size: 14px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.15);
  transform: translateY(100px);
  opacity: 0;
  transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  z-index: 9999;
}

.toast.show {
  transform: translateY(0);
  opacity: 1;
}

.sort-select {
  padding: 8px 14px;
  border-radius: 8px;
  border: 1px solid #cbd5e1;
  background-color: #ffffff;
  font-size: 13px;
  color: #334155;
  outline: none;
  cursor: pointer;
} 

/* ========================================================
   HERO SECTION MODERN ALA KAZKA & CO.
   ======================================================== */

/* ========================================================
   HERO BANNER - PERSIS GAMBAR REFERENSI
   ======================================================== */

.hero-mosaic-wrapper {
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
  padding: 0 16px;
  box-sizing: border-box;
}

/* ========================================================
   HERO BANNER - TULISAN BESAR KE TENGAH & SEPATU BESAR
   ======================================================== */

.hero-section {
  background: radial-gradient(circle at 75% 50%, #fed7aa 0%, #fdba74 50%, #fb923c 100%);
  /* Padding kiri diperbesar (90px) agar teks terdorong lebih ke tengah */
  padding: 50px 60px 50px 90px;
  border-radius: 28px;
  margin: 16px 0 36px 0;
  width: 100%;
  box-sizing: border-box;
  overflow: hidden;
  position: relative;
  min-height: 420px;
  display: flex;
  align-items: center;
}

.hero-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  gap: 30px;
}

/* 1. Teks Hero: Diberi ruang proporsional agar lebih ke arah tengah */
.hero-content {
  flex: 1.3;
  z-index: 2;
  /* Tambahan margin kiri agar semakin mengarah ke tengah */
  margin-left: 20px; 
}

/* Badge Kapsul */
.hero-badge {
  display: inline-block;
  padding: 6px 16px;
  border: 1px solid rgba(255, 255, 255, 0.7);
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(6px);
  color: #0f172a;
  font-size: 12px;
  font-weight: 800;
  letter-spacing: 0.8px;
  border-radius: 50px;
  margin-bottom: 16px;
}

/* Judul Diperbesar & Makin Tebal */
.hero-title {
  font-size: 56px; /* Diperbesar dari 46px */
  line-height: 1.05;
  font-weight: 950;
  color: #ffffff;
  margin: 0 0 12px;
  white-space: nowrap;
  letter-spacing: -0.5px;
  text-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
}

/* Sub-Judul Diperbesar */
.hero-subtitle {
  font-size: 16px; /* Diperbesar dari 14px */
  font-weight: 800;
  color: #ffffff;
  margin: 0 0 28px;
  letter-spacing: 0.4px;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

/* Tombol Explore Kapsul */
.btn-hero {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: #ffffff;
  color: #0f172a;
  padding: 14px 30px;
  border-radius: 50px;
  font-size: 14px;
  font-weight: 800;
  text-decoration: none;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
  transition: all 0.25s ease;
}

.btn-hero:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.18);
}

/* 2. Area & Gambar Sepatu: Dibuat Lebih Besar */
.hero-image-wrapper {
  flex: 1.2;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  z-index: 2;
  overflow: visible;
}

.hero-shoe {
  max-width: 620px !important; /* Diperbesar dari 520px */
  width: 145% !important;
  /* Posisi diagonal tetap dipertahankan dengan skala lebih besar */
  transform: rotate(-3deg) scale(1.12) translate(10px, -5px) !important;
  filter: drop-shadow(-25px 40px 30px rgba(194, 65, 12, 0.25));
  transition: transform 0.3s ease;
  display: block;
}

.hero-shoe:hover {
  transform: rotate(-1deg) scale(1.16) translate(10px, -10px) !important;
}

/* Responsif Mobile */
@media (max-width: 900px) {
  .hero-section {
    padding: 36px 20px;
  }
  .hero-content {
    margin-left: 0;
  }
  .hero-title {
    font-size: 36px;
    white-space: normal;
  }
  .hero-shoe {
    max-width: 340px !important;
    width: 100% !important;
    transform: none !important;
  }
}

/* ========================================================
   CATEGORY MOSAIC SECTION (DESKTOP)
   ======================================================== */

.category-mosaic-section {
  display: grid !important;
  grid-template-columns: 1.2fr 1fr 1.2fr;
  gap: 20px;
  width: 100%;
  margin: 0 0 60px 0;
  padding: 0 !important;
  box-sizing: border-box;
}

.mosaic-item {
  position: relative;
  border-radius: 12px;
  overflow: hidden;
  height: 400px;
  width: 100%;
  background: #f1f5f9;
  cursor: pointer;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
}

.mosaic-item img {
  width: 100% !important;
  height: 100% !important;
  max-width: 100%;
  object-fit: cover !important;
  display: block;
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.mosaic-item:hover img {
  transform: scale(1.05);
}

.mosaic-center-stack {
  display: flex !important;
  flex-direction: column !important;
  gap: 16px;
  height: 400px;
}

.mosaic-center-stack .mosaic-item {
  height: calc(50% - 8px) !important;
}

.mosaic-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.35);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  color: #ffffff;
  padding: 16px;
  transition: background 0.3s ease;
}

.mosaic-item:hover .mosaic-overlay {
  background: rgba(0, 0, 0, 0.45);
}

.mosaic-title {
  font-size: 26px;
  font-weight: 900;
  letter-spacing: 1.5px;
  margin: 0;
  color: #ffffff;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.6);
  text-transform: uppercase;
}

.mosaic-sub {
  font-size: 10px;
  letter-spacing: 1px;
  font-weight: 700;
  text-transform: uppercase;
  margin-top: 4px;
  opacity: 0.9;
  text-shadow: 0 1px 4px rgba(0, 0, 0, 0.6);
}

.mosaic-center-stack .mosaic-title {
  font-size: 18px;
}

.mosaic-center-stack .mosaic-sub {
  font-size: 8px;
}

/* ========================================================
   RESPONSIF MOBILE (LAYAR HP)
   ======================================================== */

@media (max-width: 768px) {
  .hero-container {
    flex-direction: column;
    text-align: center;
  }

  .hero-title {
    font-size: 32px;
  }

  .hero-shoe {
    max-width: 240px;
    transform: rotate(-10deg);
  }

  .category-mosaic-section {
    grid-template-columns: 1fr !important;
    height: auto;
    gap: 16px;
    margin-bottom: 40px;
  }

  .mosaic-item {
    height: 220px !important;
  }

  .mosaic-center-stack {
    height: auto;
    gap: 16px;
  }

  .mosaic-center-stack .mosaic-item {
    height: 160px !important;
  }

  .mosaic-title {
    font-size: 22px;
  }
}
</style>