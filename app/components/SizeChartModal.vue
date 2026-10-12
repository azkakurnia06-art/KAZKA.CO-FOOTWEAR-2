<script setup>
defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

defineEmits(['close'])
</script>

<template>
  <div class="modal" :class="{ open: isOpen }" id="sizeChartModal">
    <div class="modal-dialog">
      <div class="modal-header">
        <h3>Panduan Ukuran Sepatu (EU Sizing)</h3>
        <button class="close-btn" id="closeSizeChartBtn" @click="$emit('close')">&times;</button>
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
</template>