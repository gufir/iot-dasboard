<template>
  <div class="dashboard">
    <h1>IoT Dashboard</h1>
    <div class="cards">
      <SensorCard icon="device_thermostat" title="Temperature" :value="temperature" unit="°C" />
      <SensorCard icon="humidity_percentage" title="Humidity" :value="humidity" unit="%" />
    </div>
    <LampControl :status="lampStatus" @toggle="toggleLamp" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import SensorCard from './SensorCard.vue'
import LampControl from './LampControl.vue'

export default defineComponent({
  components: {
    SensorCard,
    LampControl,
  },
  setup() {
    const temperature = ref(25.5) //example Value
    const humidity = ref(60) //example Value
    const lampStatus = ref(false) //Example Value

    const toggleLamp = () => {
      lampStatus.value = !lampStatus.value
    }

    return { temperature, humidity, lampStatus, toggleLamp }
  },
})
</script>

<style scoped>
.dashboard {
  padding: 20px;
}

.cards {
  display: flex;
  gap: 20px;
  justify-content: center;
  margin-bottom: 20px;
}
</style>
