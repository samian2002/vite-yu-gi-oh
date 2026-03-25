<template>
  <div class="app">
    <header class="header">
      <div class="header-inner">
        <h1 class="app-title">Yu-Gi-Oh!</h1>
        <p class="app-subtitle">Card Explorer</p>
      </div>
    </header>

    <main class="main">
      <div class="controls">
        <div class="select-wrapper">
          <select v-model="selectedArchetype" @change="fetchCardsByArchetype" class="archetype-select">
            <option value="">Tutti gli archetipi</option>
            <option v-for="archetype in archetypes" :key="archetype" :value="archetype">
              {{ archetype }}
            </option>
          </select>
          <span class="select-arrow">▾</span>
        </div>
        <ResultsCount :count="cardsCount" />
      </div>

      <div v-if="loading" class="loading">
        <span class="loading-dot"></span>
        <span class="loading-dot"></span>
        <span class="loading-dot"></span>
      </div>

      <CardsList v-else />
    </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import CardsList from './components/CardsList.vue'
import ResultsCount from './components/ResultsCount.vue'
import store from './store.js'
import axios from 'axios'

const selectedArchetype = ref('')
const loading = ref(false)

const archetypes = computed(() => store.archetypes)
const cardsCount = computed(() => store.cards.length)

async function fetchArchetypes() {
  try {
    const response = await axios.get('https://db.ygoprodeck.com/api/v7/archetypes.php')
    store.archetypes = response.data.map(a => a.archetype_name)
  } catch (error) {
    console.error('Error fetching archetypes:', error)
  }
}

async function fetchCardsInBlocks(start, end) {
  const numPerRequest = 20
  store.cards = []
  loading.value = true

  for (let offset = start; offset < end; offset += numPerRequest) {
    try {
      const response = await axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?num=${numPerRequest}&offset=${offset}`)
      store.cards = store.cards.concat(response.data.data)
    } catch (error) {
      console.error('Error fetching cards:', error)
      break
    }
  }

  loading.value = false
}

async function fetchCardsByArchetype() {
  if (!selectedArchetype.value) {
    return fetchCardsInBlocks(30, 90)
  }

  loading.value = true
  try {
    const response = await axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?archetype=${selectedArchetype.value}`)
    store.cards = response.data.data
  } catch (error) {
    console.error('Error fetching cards by archetype:', error)
  }
  loading.value = false
}

onMounted(() => {
  fetchArchetypes()
  fetchCardsInBlocks(30, 90)
})
</script>

<style scoped>
.app {
  min-height: 100vh;
}

.header {
  padding: 3rem 1.5rem 2rem;
  text-align: center;
  border-bottom: 1px solid var(--border);
}

.header-inner {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}

.app-title {
  font-size: clamp(2rem, 6vw, 3.5rem);
  font-weight: 700;
  letter-spacing: -0.02em;
  background: linear-gradient(135deg, #c89b3c 0%, #f0d080 50%, #c89b3c 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  line-height: 1.1;
}

.app-subtitle {
  font-size: 0.85rem;
  font-weight: 500;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--text-muted);
}

.main {
  max-width: 1400px;
  margin: 0 auto;
  padding: 2rem 1.5rem 4rem;
}

.controls {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.select-wrapper {
  position: relative;
  display: inline-flex;
  align-items: center;
}

.archetype-select {
  appearance: none;
  background-color: var(--bg-surface);
  color: var(--text-primary);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 0.6rem 2.5rem 0.6rem 1rem;
  font-size: 0.9rem;
  font-family: inherit;
  cursor: pointer;
  transition: border-color var(--transition), background-color var(--transition);
  min-width: 220px;
}

.archetype-select:hover,
.archetype-select:focus {
  outline: none;
  border-color: var(--accent);
  background-color: var(--bg-card);
}

.select-arrow {
  position: absolute;
  right: 0.75rem;
  color: var(--text-muted);
  pointer-events: none;
  font-size: 0.8rem;
}

.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
  padding: 6rem 0;
}

.loading-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: var(--accent);
  animation: pulse 1.2s ease-in-out infinite;
}

.loading-dot:nth-child(2) { animation-delay: 0.2s; }
.loading-dot:nth-child(3) { animation-delay: 0.4s; }

@keyframes pulse {
  0%, 80%, 100% { opacity: 0.2; transform: scale(0.8); }
  40% { opacity: 1; transform: scale(1); }
}
</style>
