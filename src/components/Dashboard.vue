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
import { defineComponent, ref, onMounted, onBeforeUnmount } from 'vue'
import SensorCard from './SensorCard.vue'
import LampControl from './LampControl.vue'

export default defineComponent({
  components: {
    SensorCard,
    LampControl,
  },
  setup() {
    const temperature = ref(0)
    const humidity = ref(0)
    const lampStatus = ref(false)

    const fetchSensorData = async () => {
      try {
        const response = await fetch('http://localhost:8000/sensor-data')
        const data = await response.json()
        temperature.value = parseFloat(data.temperature)
        humidity.value = parseFloat(data.humidity)
      } catch (error) {
        console.error('failed fetching sensor data:', error)
      }
    }

    const fetchLampStatus = async () => {
      try {
        const response = await fetch('http://localhost:8000/lamp-status')
        const data = await response.json()
        lampStatus.value = data.lamp_status === '1'
      } catch (error) {
        console.error('failed fetching lamp status:', error)
      }
    }

    const toggleLamp = async () => {
      try {
        const newStatus = lampStatus.value ? '0' : '1'
        const response = await fetch('http://localhost:8000/lamp-status', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ lamp_status: newStatus }),
        })

        if (response.ok) {
          lampStatus.value = newStatus === '1'
        } else {
          console.error('Failed to toggle lamp status:', await response.text())
        }
      } catch (error) {
        console.error('Error toggling lamp status:', error)
      }
    }

    const setupWebsocket = () => {
      const ws = new WebSocket('ws://localhost:8000/realtime-status')
      ws.onmessage = (event) => {
        try {
          const data = JSON.parse(event.data)
          if (data.temperature !== undefined) {
            temperature.value = parseFloat(data.temperature)
          }
          if (data.humidity !== undefined) {
            humidity.value = parseFloat(data.humidity)
          }
          if (data.lamp_status !== undefined) {
            lampStatus.value = data.lamp_status
          }
        } catch (error) {
          console.error('Failed to parse WebSocket message:', error)
        }
      }

      ws.onerror = (error) => {
        console.error('failed connecting to websocket:', error)
      }

      ws.onclose = () => {
        console.warn('websocket connection closed. Reconnecting in 3 seconds...')
        setTimeout(() => setupWebsocket(), 3000)
      }

      return ws
    }

    let wsInstance: WebSocket | null = null

    onMounted(() => {
      fetchSensorData()
      fetchLampStatus()
      wsInstance = setupWebsocket()
    })

    onBeforeUnmount(() => {
      wsInstance?.close()
    })

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
