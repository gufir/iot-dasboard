<script setup lang="ts">
import store from '../store'
import type { User } from '@/types/user'
import Toast from 'primevue/toast'
import { useToast } from 'primevue/usetoast'
import Dashboard from '@/components/Dashboard.vue'

const toast = useToast()

const onLogout = (user: User) => {
  toast.add({
    severity: 'success',
    summary: `Goodbye, ${user.full_name}!`,
    detail: `You have successfully logged out.`,
    life: 3000,
  })
  store.clearUser()
}
</script>

<template>
  <main>
    <Toast />
    <Dashboard v-if="store.state.user" :user="store.state.user" />
  </main>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
}
</style>
