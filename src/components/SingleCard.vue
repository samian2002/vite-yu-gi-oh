<template>
  <div class="card">
    <div class="card-image-wrapper">
      <img :src="card.card_images[0].image_url" :alt="card.name" class="card-image" loading="lazy" />
    </div>
    <div class="card-body">
      <h3 class="card-name">{{ card.name }}</h3>
      <p class="card-desc" :class="{ truncated: !expanded }">{{ card.desc }}</p>
      <button class="toggle-btn" @click="expanded = !expanded">
        {{ expanded ? 'Mostra meno' : 'Leggi tutto' }}
      </button>
      <a :href="card.ygoprodeck_url" target="_blank" rel="noopener" class="card-link">
        Vedi scheda →
      </a>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

defineProps({ card: Object })

const expanded = ref(false)
</script>

<style scoped>
.card {
  background-color: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: border-color var(--transition), transform var(--transition), box-shadow var(--transition);
}

.card:hover {
  border-color: var(--border-hover);
  transform: translateY(-3px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
}

.card-image-wrapper {
  background-color: var(--bg-surface);
  overflow: hidden;
  aspect-ratio: 421 / 614;
}

.card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.3s ease;
}

.card:hover .card-image {
  transform: scale(1.03);
}

.card-body {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex: 1;
}

.card-name {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--text-primary);
  line-height: 1.3;
}

.card-desc {
  font-size: 0.78rem;
  color: var(--text-dim);
  line-height: 1.5;
  flex: 1;
}

.card-desc.truncated {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.toggle-btn {
  background: none;
  border: none;
  color: var(--accent);
  font-size: 0.75rem;
  font-weight: 500;
  padding: 0;
  text-align: left;
  transition: color var(--transition);
}

.toggle-btn:hover {
  color: var(--accent-hover);
}

.card-link {
  display: inline-block;
  margin-top: 0.25rem;
  font-size: 0.78rem;
  font-weight: 500;
  color: var(--text-muted);
  transition: color var(--transition);
}

.card-link:hover {
  color: var(--accent);
}
</style>
