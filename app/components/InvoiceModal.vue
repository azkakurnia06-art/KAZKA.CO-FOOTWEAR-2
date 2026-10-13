<script setup>
const props = defineProps({
  isOpen: Boolean,
  invoiceData: {
    type: Object,
    default: () => null
  }
})

const emit = defineEmits(['close'])

const formatRupiah = (val) => {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR',
    maximumFractionDigits: 0
  }).format(val || 0)
}

const sendWhatsAppConfirmation = () => {
  if (!props.invoiceData) return

  // Ganti dengan nomor WhatsApp tokomu (awali dengan kode negara 62)
  const adminPhone = '6281234567890'

  const itemsList = props.invoiceData.items
    .map(
      (item, index) =>
        `${index + 1}. ${item.name} (Size ${item.size}) x${item.qty} = ${formatRupiah(item.price * item.qty)}`
    )
    .join('\n')

  const message = `*KONFIRMASI PESANAN - KAZKA & CO.*
No. Invoice: #${props.invoiceData.invoiceId}
Tanggal: ${props.invoiceData.date}

*Daftar Produk:*
${itemsList}

*Detail Pengiriman:*
- Nama: ${props.invoiceData.name}
- Telepon/WA: ${props.invoiceData.phone}
- Alamat: ${props.invoiceData.address}
- Ekspedisi: ${props.invoiceData.courier}

*Detail Pembayaran:*
- Subtotal: ${formatRupiah(props.invoiceData.subtotal)}
- Ongkos Kirim: ${formatRupiah(props.invoiceData.shippingCost)}
- Diskon: -${formatRupiah(props.invoiceData.discount || 0)}
- *TOTAL BAYAR: ${formatRupiah(props.invoiceData.totalPayment)}*
- Metode: ${props.invoiceData.paymentMethod}

Halo Admin KAZKA & CO., saya sudah menyelesaikan pesanan di website dan ingin konfirmasi pembayaran.`

  const waUrl = `https://wa.me/${adminPhone}?text=${encodeURIComponent(message)}`
  window.open(waUrl, '_blank')
}

// Fungsi Cetak / Simpan ke PDF
const printInvoice = () => {
  window.print()
}
</script>

<template>
  <div v-if="isOpen && invoiceData" class="modal-overlay" @click.self="emit('close')">
    <div class="modal-box">
      <div class="modal-header">
        <div>
          <span class="badge-status">Pesanan Berhasil Dibuat</span>
          <h3 class="modal-title">Invoice #{{ invoiceData.invoiceId }}</h3>
          <p class="invoice-date">{{ invoiceData.date }}</p>
        </div>
        <button type="button" class="btn-close" @click="emit('close')">✕</button>
      </div>

      <div class="modal-body">
        <div class="invoice-summary">
          <p><strong>Penerima:</strong> {{ invoiceData.name }} ({{ invoiceData.phone }})</p>
          <p><strong>Alamat:</strong> {{ invoiceData.address }}</p>
          <p><strong>Pengiriman:</strong> {{ invoiceData.courier }}</p>
          <p><strong>Metode Pembayaran:</strong> {{ invoiceData.paymentMethod }}</p>
        </div>

        <div class="items-list">
          <div
            v-for="item in invoiceData.items"
            :key="`${item.id}-${item.size}`"
            class="item-row"
          >
            <div>
              <p class="name">{{ item.name }}</p>
              <p class="meta">EU {{ item.size }} × {{ item.qty }}</p>
            </div>
            <span class="price">{{ formatRupiah(item.price * item.qty) }}</span>
          </div>
        </div>

        <div class="price-breakdown">
          <div class="row">
            <span>Subtotal</span>
            <span>{{ formatRupiah(invoiceData.subtotal) }}</span>
          </div>
          <div class="row">
            <span>Ongkos Kirim</span>
            <span>{{ formatRupiah(invoiceData.shippingCost) }}</span>
          </div>
          <div v-if="invoiceData.discount > 0" class="row discount">
            <span>Diskon Voucher</span>
            <span>-{{ formatRupiah(invoiceData.discount) }}</span>
          </div>
          <div class="row total">
            <span>Total Bayar</span>
            <span>{{ formatRupiah(invoiceData.totalPayment) }}</span>
          </div>
        </div>
      </div>

      <div class="modal-footer">
        <button type="button" class="btn-whatsapp" @click="sendWhatsAppConfirmation">
          Konfirmasi via WhatsApp
        </button>
        <button type="button" class="btn-print" @click="printInvoice">
          Cetak / Unduh PDF
        </button>
        <button type="button" class="btn-done" @click="emit('close')">
          Tutup
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.65);
  backdrop-filter: blur(4px);
  z-index: 1100;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
}

.modal-box {
  background: #ffffff;
  border-radius: 12px;
  width: 100%;
  max-width: 480px;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  max-height: 90vh;
  overflow: hidden;
}

.modal-header {
  padding: 18px 20px;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.badge-status {
  display: inline-block;
  background: #dcfce7;
  color: #166534;
  font-size: 11px;
  font-weight: 700;
  padding: 2px 8px;
  border-radius: 4px;
  margin-bottom: 4px;
}

.modal-title {
  margin: 0;
  font-size: 17px;
  font-weight: 800;
  color: #0f172a;
}

.invoice-date {
  margin: 4px 0 0;
  font-size: 12px;
  color: #64748b;
}

.btn-close {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: #64748b;
}

.modal-body {
  padding: 20px;
  overflow-y: auto;
}

.invoice-summary {
  background: #f8fafc;
  padding: 12px;
  border-radius: 8px;
  font-size: 12px;
  color: #334155;
  margin-bottom: 16px;
  line-height: 1.6;
}

.invoice-summary p {
  margin: 0;
}

.items-list {
  border-top: 1px solid #f1f5f9;
  border-bottom: 1px solid #f1f5f9;
  padding: 10px 0;
  margin-bottom: 14px;
}

.item-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 0;
}

.item-row .name {
  margin: 0;
  font-size: 13px;
  font-weight: 600;
  color: #0f172a;
}

.item-row .meta {
  margin: 2px 0 0;
  font-size: 11px;
  color: #64748b;
}

.item-row .price {
  font-size: 13px;
  font-weight: 700;
  color: #0f172a;
}

.price-breakdown .row {
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #64748b;
  margin-bottom: 6px;
}

.price-breakdown .discount {
  color: #dc2626;
}

.price-breakdown .total {
  margin-top: 10px;
  padding-top: 8px;
  border-top: 1px solid #e2e8f0;
  font-size: 16px;
  font-weight: 800;
  color: #0f172a;
}

.modal-footer {
  padding: 16px 20px;
  border-top: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.btn-whatsapp {
  width: 100%;
  padding: 12px;
  background: #16a34a;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-weight: 700;
  font-size: 13px;
  cursor: pointer;
}

.btn-print {
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

.btn-done {
  width: 100%;
  padding: 10px;
  background: transparent;
  color: #64748b;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font-weight: 600;
  font-size: 13px;
  cursor: pointer;
}

/* Styling saat mode cetak / download PDF */
@media print {
  body * {
    visibility: hidden;
  }

  .modal-overlay {
    position: static;
    background: none;
    padding: 0;
  }

  .modal-box,
  .modal-box * {
    visibility: visible;
  }

  .modal-box {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    max-width: 100%;
    border: none;
    box-shadow: none;
  }

  .modal-footer,
  .btn-close {
    display: none !important;
  }
}
</style>