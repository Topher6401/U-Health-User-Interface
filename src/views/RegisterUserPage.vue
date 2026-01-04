<template>
  <div class="register-page">
    <h1>Registrieren</h1>
    <form @submit.prevent="onRegister">
      <div>
        <label for="name">Name</label>
        <input id="name" v-model="name" type="text" required />
      </div>
      <div>
        <label for="email">E-Mail</label>
        <input id="email" v-model="email" type="email" required />
      </div>
      <div>
        <label for="password">Passwort</label>
        <input id="password" v-model="password" type="password" required />
      </div>
      <div>
        <label for="confirm">Passwort wiederholen</label>
        <input id="confirm" v-model="passwordConfirm" type="password" required />
      </div>
      <button type="submit">Registrieren</button>
      <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const name = ref('')
const email = ref('')
const password = ref('')
const passwordConfirm = ref('')
const errorMessage = ref('')
const router = useRouter()

async function onRegister() {
  errorMessage.value = ''
  if (password.value !== passwordConfirm.value) {
    errorMessage.value = 'Passwörter stimmen nicht überein.'
    return
  }
  try {
    await axios.post('/api/register', {
      name: name.value,
      email: email.value,
      password: password.value,
    })
    // Nach erfolgreicher Registrierung eventuell zum Login weiterleiten
    router.push('/login')
  } catch (err) {
    errorMessage.value = 'Registrierung fehlgeschlagen.'
    console.error(err)
  }
}
</script>

<style scoped>
.register-page {
  max-width: 500px;
  margin: auto;
}
.error {
  color: red;
  margin-top: 1em;
}
</style>
