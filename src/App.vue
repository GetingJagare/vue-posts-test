<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header class="header pt-3 pb-3">
    <div class="container">
      <nav>
        <RouterLink to="/">Home</RouterLink>
      </nav>
    </div>
  </header>

  <main class="pt-4 pb-4">
    <div class="container">
      <RouterView />
    </div>
  </main>
</template>

<style scoped></style>
