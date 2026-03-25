<template>
  <div class="cards-grid">
    <SingleCard v-for="card in store.cards" :key="card.id" :card="card" />
  </div>
</template>

<script setup>
import SingleCard from './SingleCard.vue'
import store from '../store.js'
</script>

<style scoped>
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 1.25rem;
}
</style>
