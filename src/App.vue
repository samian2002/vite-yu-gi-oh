<template>
  <div class="container">
    <header class="my-5 text-center">
      <h1 class="app-title">YU-GI-OH</h1>
    </header>
    <ResultsCount :count="cardsCount" />
    <div class="mb-4 text-center">
      <select v-model="selectedArchetype" @change="fetchCardsByArchetype">
        <option value="">Select Archetype</option>
        <option v-for="archetype in archetypes" :key="archetype" :value="archetype">
          {{ archetype }}
        </option>
      </select>
    </div>
    <CardsList />
  </div>
</template>

<script>
import { computed } from 'vue';
import CardsList from './components/CardsList.vue';
import ResultsCount from './components/ResultsCount.vue';
import store from './store.js';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    CardsList,
    ResultsCount
  },
  data() {
    return {
      selectedArchetype: ""
    };
  },
  computed: {
    archetypes() {
      return store.archetypes;
    },
    cardsCount() {
      return store.cards.length;
    }
  },
  created() {
    this.fetchArchetypes();
    this.fetchCardsInBlocks(30, 90);
  },
  methods: {
    async fetchArchetypes() {
      try {
        const response = await axios.get('https://db.ygoprodeck.com/api/v7/archetypes.php');
        store.archetypes = response.data.map(archetype => archetype.archetype_name);
      } catch (error) {
        console.error('Error fetching archetypes:', error);
      }
    },
    async fetchCardsInBlocks(start, end) {
      const numPerRequest = 20;
      store.cards = [];

      for (let offset = start; offset < end; offset += numPerRequest) {
        try {
          const response = await axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?num=${numPerRequest}&offset=${offset}`);
          store.cards = store.cards.concat(response.data.data);
        } catch (error) {
          console.error('Error fetching cards:', error);
          break;
        }
      }
      console.log('API response:', store.cards); // Log dei dati ricevuti
    },
    async fetchCardsByArchetype() {
      if (!this.selectedArchetype) {
        return this.fetchCardsInBlocks(30, 90);
      }

      try {
        const response = await axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?archetype=${this.selectedArchetype}`);
        store.cards = response.data.data;
      } catch (error) {
        console.error('Error fetching cards by archetype:', error);
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
  background-color: #007bff;
  color: white;
  padding: 1rem;
  border-radius: 0.5rem;
  display: inline-block;
}
</style>
