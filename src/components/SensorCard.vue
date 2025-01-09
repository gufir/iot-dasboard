<script lang="ts">
import { defineComponent } from 'vue'
import type { PropType } from 'vue'

export default defineComponent({
  props: {
    title: { type: String, required: true },
    value: { type: Number, required: true },
    unit: { type: String, required: true },
    icon: { type: String },
  },
})
</script>

<template>
  <div class="sensor-card">
    <span class="material-symbols-sharp">{{ icon }}</span>
    <h2>{{ title }}</h2>
    <p>{{ value }} {{ unit }}</p>
  </div>
</template>

<style scoped>
.sensor-card {
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  width: 150px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.icon {
  font-size: 40px; /* Ukuran ikon yang konsisten */
  display: block; /* Memastikan ikon berada di baris terpisah */
  margin-bottom: 10px; /* Jarak antara ikon dan judul */
}

.title {
  font-size: 16px; /* Ukuran font judul */
  margin: 0; /* Menghapus margin default */
}

.value {
  font-size: 16px; /* Ukuran font nilai */
  margin: 0; /* Menghapus margin default */
}
</style>
