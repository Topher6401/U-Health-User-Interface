<template>
  <div class="login-page">
    <h2>Login</h2>
    <form @submit.prevent="onLogin">
      <div>
        <label for="email">E-Mail</label>
        <input id="email" type="email" v-model="email" required />
      </div>
      <div>
        <label for="password">Passwort</label>
        <input id="password" type="password" v-model="password" required />
      </div>
      <button type="submit">Einloggen</button>
      <p v-if="error" class="error">{{ error }}</p>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const email = ref('')
const password = ref('')
const error = ref('')

const router = useRouter()

async function onLogin() {
  error.value = ''
  try {
    const response = await axios.post('/api/login', {
      email: email.value,
      password: password.value
    })
    const token = response.data.token
    localStorage.setItem('token', token)
    // optional: setze in Pinia-Store, falls du Auth-State global speichern willst
    router.push('/timeslots')  // nach dem Login weiterleiten
  } catch (e) {
    error.value = 'Login fehlgeschlagen. Bitte versuche es erneut.'
    console.error(e)
  }
}
</script>

<style scoped>
.login-page {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.login-page label {
  display: block;
  margin-bottom: 0.5rem;
}

.login-page input {
  width: 100%;
  padding: 0.5rem;
  margin-bottom: 1rem;
}

.login-page button {
  padding: 0.6rem 1.2rem;
}

.error {
  color: red;
  margin-top: 1rem;
}
</style>
