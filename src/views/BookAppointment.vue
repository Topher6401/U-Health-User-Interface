<template>
  <div class="book-page">
    <h1>Termin Buchen</h1>
    <div v-if="!slot">Lade Termin …</div>
    <div v-else>
      <p>Datum: {{ slot.date }}</p>
      <p>Von: {{ slot.start }} bis {{ slot.end }}</p>

      <form @submit.prevent="onBook">
        <div>
          <label for="reason">Grund / Beschreibung</label>
          <textarea id="reason" v-model="reason" required></textarea>
        </div>
        <button type="submit">Buchen</button>
        <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const slot = ref(null)
const reason = ref('')
const errorMessage = ref('')

onMounted(async () => {
  const slotId = route.params.slotId
  try {
    const token = localStorage.getItem('token')
    const response = await axios.get(`/api/timeslots/${slotId}`, {
      headers: {
        Authorization: `Bearer ${token}`
      }
    })
    slot.value = response.data
  } catch (err) {
    errorMessage.value = 'Fehler beim Laden des Zeitfensters.'
    console.error(err)
  }
})

async function onBook() {
  errorMessage.value = ''
  try {
    const token = localStorage.getItem('token')
    await axios.post('/api/appointments', {
      slotId: slot.value.id,
      reason: reason.value
    }, {
      headers: { Authorization: `Bearer ${token}` }
    })
    // Nach erfolgreicher Buchung z. B. zurück zur Übersicht
    router.push('/timeslots')
  } catch (err) {
    errorMessage.value = 'Fehler beim Buchen des Termins.'
    console.error(err)
  }
}
</script>

<style scoped>
.book-page {
  max-width: 500px;
  margin: auto;
}
.error {
  color: red;
  margin-top: 1em;
}
</style>
