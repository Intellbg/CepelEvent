<script setup lang="ts">
import EEForm from './components/EEForm.vue'

let g1 = new Date();
let g2 = new Date("2023-01-01");
if (g1.getTime() > g2.getTime()) {
  window.location.href = "http://cepelee.com"
}
</script>

<template>

  <main>
    <div class="container is-centered">
      <EEForm />
    </div>
  </main>
</template>

