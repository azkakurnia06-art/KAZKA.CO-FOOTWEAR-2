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
  const adminPhone = '6281234567890' // Ganti nomor admin Anda

  const itemsList = props.invoiceData.items
    .map((item, index) => `${index + 1}. ${item.name} (Size ${item.size}) x${item.qty} = ${formatRupiah(item.price * item.qty)}`)
    .join('\n')

  const message = `*KONFIRMASI PESANAN - KAZKA & CO.*
No. Invoice: #${props.invoiceData.invoiceId}
Tanggal: ${props.invoiceData.date}

*Daftar Produk:*
${itemsList}

*Detail Pengiriman:*
- Nama: ${props.invoiceData.name}
- Telepon: ${props.invoiceData.phone}
- Alamat: ${props.invoiceData.address}
- Kurir: ${props.invoiceData.courier}

*Total Bayar: ${formatRupiah(props.invoiceData.totalPayment)}*
Metode: ${props.invoiceData.paymentMethod}

Halo Admin KAZKA & CO., saya ingin konfirmasi pesanan ini.`

  const waUrl = `https://wa.me/${adminPhone}?text=${encodeURIComponent(message)}`
  window.open(waUrl, '_blank')
}

const printInvoice = () => {
  window.print()
}
</script>

<template>
  <div v-if="isOpen && invoiceData" class="modal-overlay" @click.self="emit('close')">
    <div class="modal-box">
      <!-- Aksi di Layar Komputer -->
      <div class="action-bar no-print">
        <button type="button" class="btn-action btn-wa" @click="sendWhatsAppConfirmation">
          Konfirmasi WhatsApp
        </button>
        <button type="button" class="btn-action btn-print" @click="printInvoice">
          Cetak / Unduh PDF
        </button>
        <button type="button" class="btn-action btn-close" @click="emit('close')">✕</button>
      </div>

      <!-- Kertas Invoice Resmi -->
      <div class="invoice-paper">
        <!-- Header -->
        <div class="inv-header">
          <div class="brand-info">
            <h1 class="brand-title">KAZKA & CO.</h1>
            <p class="brand-sub">Premium Footwear & Apparel</p>
            <p class="brand-sub">Official Online Store</p>
          </div>
          <div class="inv-title-area">
            <h2 class="inv-title">INVOICE</h2>
            <p class="inv-number">#INV/{{ invoiceData.invoiceId }}</p>
            <span class="badge-paid">PESANAN TERKONFIRMASI</span>
          </div>
        </div>

        <hr class="divider-line" />

        <!-- Info Pembeli & Detail Pengiriman -->
        <div class="info-grid">
          <div class="info-card">
            <h4 class="card-heading">INFORMASI PEMBELI</h4>
            <div class="info-row">
              <span class="label">Nama:</span>
              <span class="val font-bold">{{ invoiceData.name }}</span>
            </div>
            <div class="info-row">
              <span class="label">Telepon:</span>
              <span class="val">{{ invoiceData.phone }}</span>
            </div>
            <div class="info-row">
              <span class="label">Alamat:</span>
              <span class="val">{{ invoiceData.address }}</span>
            </div>
          </div>

          <div class="info-card">
            <h4 class="card-heading">DETAIL PENGIRIMAN & PEMBAYARAN</h4>
            <div class="info-row">
              <span class="label">Tanggal:</span>
              <span class="val font-bold">{{ invoiceData.date }}</span>
            </div>
            <div class="info-row">
              <span class="label">Ekspedisi:</span>
              <span class="val">{{ invoiceData.courier }}</span>
            </div>
            <div class="info-row">
              <span class="label">Pembayaran:</span>
              <span class="val">{{ invoiceData.paymentMethod }}</span>
            </div>
          </div>
        </div>

        <!-- Tabel Produk ala iBox -->
        <table class="inv-table">
          <thead>
            <tr>
              <th style="width: 5%">NO</th>
              <th style="width: 20%">SKU</th>
              <th style="width: 45%">DESKRIPSI PRODUK</th>
              <th style="width: 10%; text-align: center;">QTY</th>
              <th style="width: 20%; text-align: right;">HARGA (RP)</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, idx) in invoiceData.items" :key="`${item.id}-${item.size}`">
              <td>{{ idx + 1 }}</td>
              <td class="sku-text">KZK-00{{ item.id }}</td>
              <td>
                <div class="item-name">{{ item.name }}</div>
                <div class="item-desc">Ukuran: EU {{ item.size }}</div>
              </td>
              <td style="text-align: center;">{{ item.qty }} Pcs</td>
              <td style="text-align: right;" class="font-bold">
                {{ formatRupiah(item.price * item.qty) }}
              </td>
            </tr>
          </tbody>
        </table>

        <!-- Ringkasan Total -->
        <div class="summary-section">
          <div class="summary-table">
            <div class="sum-row">
              <span>Subtotal:</span>
              <span>{{ formatRupiah(invoiceData.subtotal) }}</span>
            </div>
            <div class="sum-row">
              <span>Ongkos Kirim:</span>
              <span>{{ formatRupiah(invoiceData.shippingCost) }}</span>
            </div>
            <div v-if="invoiceData.discount > 0" class="sum-row discount">
              <span>Diskon:</span>
              <span>-{{ formatRupiah(invoiceData.discount) }}</span>
            </div>
            <div class="sum-row grand-total">
              <span>Total Pembayaran:</span>
              <span>{{ formatRupiah(invoiceData.totalPayment) }}</span>
            </div>
          </div>
        </div>

        <!-- Catatan Syarat & Ketentuan -->
        <div class="terms-section">
          <p class="terms-title">Catatan & Ketentuan Pembelian:</p>
          <ul>
            <li>Simpan bukti invoice ini sebagai bukti resmi transaksi di KAZKA & CO.</li>
            <li>Penukaran ukuran (size exchange) berlaku maksimal 2x24 jam sejak barang diterima dengan kondisi baru dan tag utuh.</li>
            <li>Nomor resi pengiriman akan diperbarui otomatis melalui konfirmasi WhatsApp Admin.</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.65);
  backdrop-filter: blur(4px);
  z-index: 1200;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
  overflow-y: auto;
}

.modal-box {
  background: #ffffff;
  border-radius: 10px;
  width: 100%;
  max-width: 680px;
  max-height: 92vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
}

.action-bar {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 8px;
  padding: 12px 20px;
  background: #f8fafc;
  border-bottom: 1px solid #e2e8f0;
}

.btn-action {
  padding: 7px 14px;
  border-radius: 6px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  border: none;
}

.btn-wa { background: #16a34a; color: white; }
.btn-print { background: #0f172a; color: white; }
.btn-close { background: #e2e8f0; color: #475569; font-size: 14px; }

.invoice-paper {
  padding: 28px 32px;
  overflow-y: auto;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  color: #0f172a;
}

.inv-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.brand-title {
  margin: 0;
  font-size: 24px;
  font-weight: 900;
  letter-spacing: -0.5px;
}

.brand-sub {
  margin: 2px 0 0;
  font-size: 11px;
  color: #64748b;
}

.inv-title-area {
  text-align: right;
}

.inv-title {
  margin: 0;
  font-size: 22px;
  font-weight: 900;
  letter-spacing: 0.5px;
}

.inv-number {
  margin: 2px 0 6px;
  font-size: 12px;
  color: #475569;
  font-weight: 600;
}

.badge-paid {
  display: inline-block;
  background: #dcfce7;
  color: #15803d;
  font-size: 10px;
  font-weight: 800;
  padding: 3px 8px;
  border-radius: 4px;
  letter-spacing: 0.3px;
}

.divider-line {
  border: none;
  border-top: 1px solid #e2e8f0;
  margin: 18px 0;
}

.info-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  margin-bottom: 20px;
}

.info-card {
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  padding: 12px;
  background: #ffffff;
}

.card-heading {
  margin: 0 0 8px;
  font-size: 11px;
  font-weight: 800;
  color: #334155;
  border-bottom: 1px dashed #cbd5e1;
  padding-bottom: 4px;
}

.info-row {
  display: flex;
  font-size: 11px;
  margin-bottom: 4px;
  line-height: 1.4;
}

.info-row .label {
  width: 75px;
  color: #64748b;
  flex-shrink: 0;
}

.info-row .val {
  color: #0f172a;
}

.font-bold { font-weight: 700; }

.inv-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 16px;
  font-size: 11px;
}

.inv-table th {
  background: #0f172a;
  color: #ffffff;
  padding: 8px 10px;
  font-weight: 700;
  text-align: left;
  letter-spacing: 0.3px;
}

.inv-table td {
  padding: 10px;
  border-bottom: 1px solid #f1f5f9;
}

.sku-text {
  color: #64748b;
  font-family: monospace;
}

.item-name {
  font-weight: 700;
  color: #0f172a;
}

.item-desc {
  color: #64748b;
  font-size: 10px;
}

.summary-section {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 20px;
}

.summary-table {
  width: 260px;
}

.sum-row {
  display: flex;
  justify-content: space-between;
  font-size: 11px;
  margin-bottom: 4px;
  color: #475569;
}

.sum-row.discount { color: #dc2626; }

.sum-row.grand-total {
  border-top: 2px solid #0f172a;
  padding-top: 6px;
  margin-top: 6px;
  font-size: 13px;
  font-weight: 900;
  color: #0f172a;
}

.terms-section {
  border-left: 3px solid #0f172a;
  padding-left: 12px;
  font-size: 10px;
  color: #64748b;
  line-height: 1.5;
}

.terms-title {
  margin: 0 0 4px;
  font-weight: 700;
  color: #334155;
}

.terms-section ul {
  margin: 0;
  padding-left: 14px;
}

/* Format Khusus Cetak / PDF */
@media print {
  .no-print { display: none !important; }
  body * { visibility: hidden; }
  .invoice-paper, .invoice-paper * { visibility: visible; }
  .modal-overlay {
    position: static;
    padding: 0;
    background: none;
  }
  .modal-box {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    max-width: 100%;
    box-shadow: none;
    border: none;
  }
}

@media (max-width: 600px) {
  .info-grid { grid-template-columns: 1fr; }
  .invoice-paper { padding: 18px; }
}
</style>