<script setup lang="ts">
import { useRouter } from 'vue-router';

const router = useRouter();

function goToLogin() {
  router.push('/login');
}
</script>


<template>
  <div class="home">
    <h1>Welcome to U‑Health!</h1>
    <p>
      Visit
      <a href="https://vuejs.org/" target="_blank" rel="noopener">
        vuejs.org
      </a>
      to read the documentation.
    </p>

    <button @click="goToLogin">Go to Login</button>
  </div>
</template>

<style scoped>
.home {
  text-align: center;
  margin-top: 2rem;
}

button {
  padding: 0.6rem 1.2rem;
  font-size: 1rem;
}
</style>
