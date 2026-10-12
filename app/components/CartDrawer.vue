<script setup>
defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  cart: {
    type: Array,
    required: true
  },
  cartTotal: {
    type: Number,
    required: true
  },
  customerName: {
    type: String,
    default: ''
  },
  customerEmail: {
    type: String,
    default: ''
  },
  customerPhone: {
    type: String,
    default: ''
  }
})

defineEmits([
  'close',
  'change-qty',
  'checkout',
  'update:customerName',
  'update:customerEmail',
  'update:customerPhone'
])

const formatRupiah = (val) => {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR',
    maximumFractionDigits: 0
  }).format(val)
}
</script>

<template>
  <aside class="cart-drawer" :class="{ open: isOpen }">
    <div class="cart-header">
      <h3>Keranjang Belanja</h3>
      <button class="close-btn" @click="$emit('close')">&times;</button>
    </div>

    <div class="cart-items">
      <p v-if="cart.length === 0" class="empty-msg">Keranjang masih kosong.</p>
      <div v-for="item in cart" :key="item.id + '-' + item.size" class="cart-item">
        <div class="cart-item-detail">
          <strong>{{ item.name }}</strong>
          <span class="cart-item-meta">Ukuran: EU {{ item.size }} | {{ formatRupiah(item.price) }}</span>
        </div>
        <div class="cart-qty-ctrl">
          <button type="button" @click="$emit('change-qty', item.id, item.size, -1)">-</button>
          <span>{{ item.qty }}</span>
          <button type="button" @click="$emit('change-qty', item.id, item.size, 1)">+</button>
        </div>
      </div>
    </div>

    <div class="cart-footer">
      <div class="customer-form">
        <label class="form-label">Nama Lengkap</label>
        <input
          :value="customerName"
          @input="$emit('update:customerName', $event.target.value)"
          type="text"
          class="form-input"
          placeholder="Contoh: Azka Kurnia"
        >

        <label class="form-label">Email</label>
        <input
          :value="customerEmail"
          @input="$emit('update:customerEmail', $event.target.value)"
          type="email"
          class="form-input"
          placeholder="Contoh: nama@email.com"
        >

        <label class="form-label">Nomor WhatsApp</label>
        <input
          :value="customerPhone"
          @input="$emit('update:customerPhone', $event.target.value)"
          type="tel"
          class="form-input"
          placeholder="Contoh: 08123456789"
        >
      </div>

      <div class="total-row">
        <span>Total:</span>
        <strong>{{ formatRupiah(cartTotal) }}</strong>
      </div>
      <button class="checkout-btn" @click="$emit('checkout')">Lanjut ke Pembayaran</button>
    </div>
  </aside>
</template>