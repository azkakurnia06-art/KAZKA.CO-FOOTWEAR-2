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
  }
])

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

    <!-- Main Content -->
    <main class="main-content">
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
  gap: 8px;
  overflow-x: auto;
  padding-bottom: 4px;
}

.category-tabs button {
  padding: 8px 18px;
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 500;
  color: #475569;
  cursor: pointer;
  white-space: nowrap;
  transition: all 0.2s ease;
}

.category-tabs button.active {
  background-color: #0f172a;
  color: #ffffff;
  border-color: #0f172a;
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
</style>