<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Shortener from "./components/Shortener/Shortener.vue";

</script>

<template>
  <header>

    <div>
      <Shortener/>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
</style>
