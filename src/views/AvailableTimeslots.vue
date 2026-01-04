<template>
  <div class="timeslots-page">
    <h1>Verfügbare Termine</h1>
    <div v-if="loading">Lade freie Zeitfenster...</div>
    <div v-else>
      <ul>
        <li v-for="slot in timeslots" :key="slot.id">
          {{ slot.date }} – {{ slot.start }} bis {{ slot.end }}
          <button @click="book(slot)">Buchen</button>
        </li>
      </ul>
      <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const timeslots = ref([])
const loading = ref(true)
const errorMessage = ref('')
const router = useRouter()

onMounted(async () => {
  await loadTimeslots()
})

async function loadTimeslots() {
  loading.value = true
  errorMessage.value = ''
  try {
    const token = localStorage.getItem('token')
    const response = await axios.get('/api/timeslots', {
      headers: {
        Authorization: `Bearer ${token}`
      }
    })
    timeslots.value = response.data
  } catch (err) {
    errorMessage.value = 'Fehler beim Laden der Zeitfenster.'
    console.error(err)
  } finally {
    loading.value = false
  }
}

function book(slot) {
  // z. B. zur Buchungsseite navigieren und Slot übergeben
  router.push({ name: 'BookAppointment', params: { slotId: slot.id } })
}
</script>

<style scoped>
.timeslots-page {
  max-width: 600px;
  margin: auto;
}
.error {
  color: red;
}
</style>
