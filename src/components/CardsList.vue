<template>
    <div class="row">
      <SingleCard v-for="(card, index) in store.cards" :key="index" :card="card" />
    </div>
  </template>
  
  <script>
  import SingleCard from './SingleCard.vue';
  import store from '../store.js';
  
  export default {
    name: 'CardsList',
    components: {
      SingleCard
    },
    data() {
      return {
        store
      }
    }
  }
  </script>
  