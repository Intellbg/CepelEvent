<script setup lang="ts">
import EEForm from './components/EEForm.vue'
const send = localStorage.getItem("send")
</script>

<template>

  <main>
    <div class="container is-centered">
      <EEForm />
    </div>
  </main>
</template>

