<script lang="ts">
import { defineComponent } from 'vue'
import lampOn from '@/assets/lamp-on.svg'
import lampOff from '@/assets/lamp-off.svg'

export default defineComponent({
  props: {
    status: {
      type: Boolean,
      required: true,
    },
  },
  computed: {
    lampIcon() {
      return this.status ? lampOn : lampOff
    },
  },
})
</script>

<template>
  <div class="lamp-control">
    <h2>Lamp Control</h2>
    <div class="status-icon">
      <img :src="lampIcon" alt="Lamp Status" class="lamp-icon" />
    </div>
    <div class="status">
      <p>Status: {{ status ? 'On' : 'Off' }}</p>
    </div>
    <button @click="$emit('toggle')">
      {{ status ? 'Turn Off' : 'Turn On' }}
    </button>
  </div>
</template>

<style scoped>
.lamp-control {
  text-align: center;
}

.status-icon {
  display: center;
  align-items: center;
  justify-content: center;
}

.lamp-icon {
  width: 100px;
  height: 100px;
  align-self: center;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #0056b3;
}
</style>
