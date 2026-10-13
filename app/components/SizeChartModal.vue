<script setup>
defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

defineEmits(['close'])

const chartData = [
  { eu: 38, foot: '24.0 cm', insole: '24.7 cm' },
  { eu: 39, foot: '24.5 cm', insole: '25.3 cm' },
  { eu: 40, foot: '25.5 cm', insole: '26.0 cm' },
  { eu: 41, foot: '26.0 cm', insole: '26.6 cm' },
  { eu: 42, foot: '27.0 cm', insole: '27.3 cm' },
  { eu: 43, foot: '27.5 cm', insole: '28.0 cm' },
  { eu: 44, foot: '28.5 cm', insole: '28.7 cm' }
]
</script>

<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-card">
      <div class="modal-header">
        <h3 class="modal-title">Panduan Ukuran Sepatu (EU)</h3>
        <button class="close-btn" @click="$emit('close')">&times;</button>
      </div>

      <div class="modal-body">
        <p class="guide-note">
          Ukur panjang telapak kaki dari tumit hingga ujung jari terpanjang dalam satuan centimeter (cm).
        </p>

        <div class="table-wrapper">
          <table class="size-table">
            <thead>
              <tr>
                <th>Ukuran (EU)</th>
                <th>Panjang Kaki (cm)</th>
                <th>Insole (cm)</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="row in chartData" :key="row.eu">
                <td><strong>{{ row.eu }}</strong></td>
                <td>{{ row.foot }}</td>
                <td>{{ row.insole }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <p class="tips">
          💡 <strong>Tips:</strong> Jika tipe kaki Anda lebar (<em>wide foot</em>), disarankan memilih 1 nomor di atas ukuran standar.
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 16px;
  backdrop-filter: blur(2px);
}

.modal-card {
  background: #ffffff;
  width: 100%;
  max-width: 520px;
  border-radius: 12px;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  animation: popIn 0.2s ease-out;
}

@keyframes popIn {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}

.modal-header {
  padding: 16px 20px;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-title {
  font-size: 16px;
  font-weight: 700;
  color: #0f172a;
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  color: #64748b;
  cursor: pointer;
  line-height: 1;
}

.modal-body {
  padding: 20px;
}

.guide-note {
  font-size: 13px;
  color: #475569;
  margin-bottom: 16px;
}

.table-wrapper {
  overflow-x: auto;
  margin-bottom: 16px;
}

.size-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  text-align: left;
}

.size-table th {
  background-color: #f8fafc;
  padding: 10px 12px;
  border-bottom: 2px solid #e2e8f0;
  color: #334155;
}

.size-table td {
  padding: 10px 12px;
  border-bottom: 1px solid #e2e8f0;
  color: #475569;
}

.tips {
  font-size: 12px;
  color: #64748b;
  background-color: #f1f5f9;
  padding: 10px 14px;
  border-radius: 6px;
}
</style>