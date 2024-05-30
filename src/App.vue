<template>
  <div class="container">
    <header class="my-5 text-center">
      <h1 class="app-title">YU-GI-OH</h1>
    </header>
    <CardsList />
  </div>
</template>

<script>
import CardsList from './components/CardsList.vue';
import store from './store.js';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    CardsList
  },
  created() {
    this.fetchCardsInBlocks(30, 90); //30esima alla 90esima
  },
  methods: {
    async fetchCardsInBlocks(start, end) {
      const numPerRequest = 20;
      const requests = [];
      for (let offset = start; offset < end; offset += numPerRequest) {
        requests.push(axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?num=${numPerRequest}&offset=${offset}`));
      }
      try {
        const responses = await Promise.all(requests);
        const cards = responses.flatMap(response => response.data.data);
        console.log('API response:', cards); // Log dei dati ricevuti
        store.cards = cards;
      } catch (error) {
        console.error('Error fetching cards:', error);
      }
    }
  }
}
</script>

<style>
body {
  background-color: #343a40;
  color: #f8f9fa;
}

.app-title {
  background-color: #ff0a0a;
  color: white;
  padding: 1rem;
  border-radius: 0.5rem;
  display: inline-block;
}
</style>
