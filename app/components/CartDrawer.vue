<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  isOpen: Boolean,
  cart: {
    type: Array,
    default: () => []
  },
  cartTotal: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits(['close', 'change-qty', 'checkout'])

// Data Form Pengiriman & Pembayaran
const customerName = ref('')
const customerEmail = ref('')
const customerPhone = ref('')
const customerAddress = ref('')
const customerCity = ref('')
const customerPostalCode = ref('')

const selectedCourier = ref('jne')
const shippingCosts = {
  jne: 20000,
  jnt: 22000,
  sicepat: 19000
}

const paymentMethod = ref('bca') // bca, mandiri, qris

// Kupon Diskon
const couponCode = ref('')
const discountAmount = ref(0)
const couponError = ref('')
const couponSuccess = ref('')

const applyCoupon = () => {
  couponError.value = ''
  couponSuccess.value = ''
  const code = couponCode.value.trim().toUpperCase()

  if (code === 'DISKONKAZKA') {
    discountAmount.value = 30000
    couponSuccess.value = 'Kupon berhasil digunakan! Diskon Rp 30.000'
  } else if (code === '') {
    discountAmount.value = 0
  } else {
    discountAmount.value = 0
    couponError.value = 'Kode kupon tidak valid'
  }
}

const currentShippingCost = computed(() => {
  return props.cart.length > 0 ? (shippingCosts[selectedCourier.value] || 0) : 0
})

const finalTotal = computed(() => {
  const base = props.cartTotal + currentShippingCost.value - discountAmount.value
  return base > 0 ? base : 0
})

const formatRupiah = (val) => {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR',
    maximumFractionDigits: 0
  }).format(val)
}

const handleCheckoutSubmit = () => {
  if (!customerName.value || !customerPhone.value || !customerAddress.value || !customerCity.value) {
    alert('Harap lengkapi Nama, WhatsApp, Alamat Jalan, dan Kota terlebih dahulu.')
    return
  }

  emit('checkout', {
    name: customerName.value,
    email: customerEmail.value,
    phone: customerPhone.value,
    address: `${customerAddress.value}, ${customerCity.value} ${customerPostalCode.value}`.trim(),
    courier: selectedCourier.value.toUpperCase(),
    shippingCost: currentShippingCost.value,
    paymentMethod: paymentMethod.value.toUpperCase(),
    discount: discountAmount.value,
    totalPayment: finalTotal.value
  })
}
</script>

<template>
  <div v-if="isOpen" class="drawer-overlay" @click.self="emit('close')">
    <div class="drawer-panel">
      <div class="drawer-header">
        <h3 class="drawer-title">Keranjang Belanja</h3>
        <button type="button" class="btn-close" @click="emit('close')">✕</button>
      </div>

      <div class="drawer-body">
        <div v-if="cart.length === 0" class="empty-cart">
          <p>Keranjang belanja masih kosong.</p>
        </div>

        <div v-else>
          <!-- Daftar Barang -->
          <div class="cart-items">
            <div v-for="item in cart" :key="`${item.id}-${item.size}`" class="cart-item">
              <img :src="item.image" :alt="item.name" class="item-img" />
              <div class="item-info">
                <h4 class="item-name">{{ item.name }}</h4>
                <p class="item-meta">Ukuran: EU {{ item.size }}</p>
                <p class="item-price">{{ formatRupiah(item.price) }}</p>
                <div class="qty-control">
                  <button type="button" @click="emit('change-qty', { id: item.id, size: item.size, delta: -1 })">-</button>
                  <span>{{ item.qty }}</span>
                  <button type="button" @click="emit('change-qty', { id: item.id, size: item.size, delta: 1 })">+</button>
                </div>
              </div>
            </div>
          </div>

          <!-- Form Checkout Lengkap -->
          <div class="checkout-form">
            <h4 class="section-heading">Informasi Penerima</h4>
            <input v-model="customerName" type="text" placeholder="Nama Lengkap *" class="form-input" />
            <input v-model="customerPhone" type="tel" placeholder="Nomor WhatsApp *" class="form-input" />
            <input v-model="customerEmail" type="email" placeholder="Email (Opsional)" class="form-input" />

            <h4 class="section-heading">Alamat Pengiriman</h4>
            <textarea v-model="customerAddress" placeholder="Nama Jalan, RT/RW, Kelurahan, Kecamatan *" rows="2" class="form-input"></textarea>
            <div class="form-row">
              <input v-model="customerCity" type="text" placeholder="Kota / Kab *" class="form-input" />
              <input v-model="customerPostalCode" type="text" placeholder="Kode Pos" class="form-input" />
            </div>

            <h4 class="section-heading">Ekspedisi</h4>
            <select v-model="selectedCourier" class="form-input">
              <option value="jne">JNE Reguler (Rp 20.000)</option>
              <option value="jnt">J&T Express (Rp 22.000)</option>
              <option value="sicepat">SiCepat REG (Rp 19.000)</option>
            </select>

            <h4 class="section-heading">Metode Pembayaran</h4>
            <select v-model="paymentMethod" class="form-input">
              <option value="bca">Transfer Bank - BCA</option>
              <option value="mandiri">Transfer Bank - Mandiri</option>
              <option value="qris">QRIS (GoPay, ShopeePay, DANA)</option>
            </select>

            <h4 class="section-heading">Kupon Diskon</h4>
            <div class="coupon-box">
              <input v-model="couponCode" type="text" placeholder="Gunakan DISKONKAZKA" class="form-input coupon-input" />
              <button type="button" class="btn-coupon" @click="applyCoupon">Pakai</button>
            </div>
            <p v-if="couponSuccess" class="coupon-msg success">{{ couponSuccess }}</p>
            <p v-if="couponError" class="coupon-msg error">{{ couponError }}</p>
          </div>
        </div>
      </div>

      <!-- Ringkasan & Submit -->
      <div v-if="cart.length > 0" class="drawer-footer">
        <div class="price-row">
          <span>Subtotal</span>
          <span>{{ formatRupiah(cartTotal) }}</span>
        </div>
        <div class="price-row">
          <span>Ongkos Kirim</span>
          <span>{{ formatRupiah(currentShippingCost) }}</span>
        </div>
        <div v-if="discountAmount > 0" class="price-row discount">
          <span>Diskon Voucher</span>
          <span>- {{ formatRupiah(discountAmount) }}</span>
        </div>
        <div class="price-row total">
          <span>Total Pembayaran</span>
          <span>{{ formatRupiah(finalTotal) }}</span>
        </div>

        <button type="button" class="btn-checkout" @click="handleCheckoutSubmit">
          Lanjut ke Pembayaran
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.drawer-overlay {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.6);
  backdrop-filter: blur(4px);
  z-index: 1000;
  display: flex;
  justify-content: flex-end;
}

.drawer-panel {
  width: 100%;
  max-width: 440px;
  height: 100%;
  background: #ffffff;
  display: flex;
  flex-direction: column;
}

.drawer-header {
  padding: 18px 20px;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.drawer-title {
  margin: 0;
  font-size: 16px;
  font-weight: 700;
  color: #0f172a;
}

.btn-close {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: #64748b;
}

.drawer-body {
  padding: 20px;
  overflow-y: auto;
  flex: 1;
}

.empty-cart {
  text-align: center;
  color: #94a3b8;
  margin-top: 40px;
}

.cart-items {
  display: flex;
  flex-direction: column;
  gap: 14px;
  margin-bottom: 24px;
}

.cart-item {
  display: flex;
  gap: 12px;
  padding-bottom: 14px;
  border-bottom: 1px solid #f1f5f9;
}

.item-img {
  width: 64px;
  height: 64px;
  border-radius: 8px;
  object-fit: cover;
  background: #f8fafc;
}

.item-info {
  flex: 1;
}

.item-name {
  margin: 0 0 4px;
  font-size: 13px;
  font-weight: 600;
  color: #0f172a;
}

.item-meta {
  margin: 0 0 4px;
  font-size: 11px;
  color: #64748b;
}

.item-price {
  margin: 0 0 8px;
  font-size: 13px;
  font-weight: 700;
  color: #16a34a;
}

.qty-control {
  display: inline-flex;
  align-items: center;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  overflow: hidden;
}

.qty-control button {
  background: #f8fafc;
  border: none;
  padding: 2px 10px;
  cursor: pointer;
  font-weight: 700;
}

.qty-control span {
  padding: 0 10px;
  font-size: 12px;
}

.checkout-form {
  border-top: 2px dashed #e2e8f0;
  padding-top: 18px;
}

.section-heading {
  margin: 14px 0 8px;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  font-weight: 700;
  color: #475569;
}

.form-input {
  width: 100%;
  padding: 9px 12px;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  font-size: 13px;
  margin-bottom: 8px;
  box-sizing: border-box;
}

.form-row {
  display: flex;
  gap: 8px;
}

.coupon-box {
  display: flex;
  gap: 8px;
}

.coupon-input {
  margin-bottom: 0;
}

.btn-coupon {
  padding: 0 16px;
  background: #0f172a;
  color: #ffffff;
  border: none;
  border-radius: 6px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
}

.coupon-msg {
  margin: 6px 0 0;
  font-size: 11px;
}

.coupon-msg.success {
  color: #16a34a;
}

.coupon-msg.error {
  color: #dc2626;
}

.drawer-footer {
  padding: 18px 20px;
  border-top: 1px solid #e2e8f0;
  background: #f8fafc;
}

.price-row {
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #64748b;
  margin-bottom: 6px;
}

.price-row.discount {
  color: #dc2626;
}

.price-row.total {
  font-size: 15px;
  font-weight: 800;
  color: #0f172a;
  margin-top: 10px;
  padding-top: 8px;
  border-top: 1px solid #cbd5e1;
}

.btn-checkout {
  width: 100%;
  margin-top: 14px;
  padding: 12px;
  background: #0f172a;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-weight: 700;
  font-size: 14px;
  cursor: pointer;
}
</style>