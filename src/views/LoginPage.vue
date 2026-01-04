<template>
  <div class="login-page">
    <h1>Login</h1>
    <form @submit.prevent="onSubmit">
      <div>
        <label for="email">E-Mail</label>
        <input id="email" v-model="email" type="email" required />
      </div>
      <div>
        <label for="password">Passwort</label>
        <input id="password" v-model="password" type="password" required />
      </div>
      <button type="submit">Einloggen</button>
      <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const email = ref('')
const password = ref('')
const errorMessage = ref('')
const router = useRouter()

async function onSubmit() {
  errorMessage.value = ''
  try {
    const response = await axios.post('/api/login', {
      email: email.value,
      password: password.value,
    })
    // z. B. Token speichern
    const token = response.data.token
    localStorage.setItem('token', token)
    // nach Login weiterleiten, z. B. zur Startseite
    router.push('/timeslots')
  } catch (err) {
    errorMessage.value = 'Login fehlgeschlagen. Bitte prüfen Sie Ihre Eingaben.'
    console.error(err)
  }
}
</script>

<style scoped>
.login-page {
  max-width: 400px;
  margin: auto;
}
.error {
  color: red;
  margin-top: 1em;
}
</style>
