<template>
  <span class="results-count">{{ count }} carte</span>
</template>

<script setup>
defineProps({
  count: {
    type: Number,
    required: true
  }
})
</script>

<style scoped>
.results-count {
  font-size: 0.85rem;
  font-weight: 500;
  color: var(--text-muted);
  letter-spacing: 0.03em;
}
</style>
