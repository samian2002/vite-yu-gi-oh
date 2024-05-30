<template>
  <div class="col-md-4 col-lg-3 mb-4">
    <div class="card h-100">
      <img :src="card.card_images[0].image_url" class="card-img-top" alt="Card Image">
      <div class="card-body">
        <h5 class="card-title">{{ card.name }}</h5>
        <p class="card-text" :class="{ 'truncate': !expanded }">{{ card.desc }}</p>
        <button @click="toggleExpanded" class="btn btn-link p-0">{{ expanded ? 'Read less' : 'Read more' }}</button>
        <a :href="card.ygoprodeck_url" class="btn btn-primary mt-2">Go to card</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SingleCard',
  props: {
    card: Object
  },
  data() {
    return {
      expanded: false
    }
  },
  methods: {
    toggleExpanded() {
      this.expanded = !this.expanded;
    }
  }
}
</script>

<style scoped>
.card {
  overflow: hidden;
}

.card-body {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.card-text.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  max-height: 3rem;
}

.card .btn-link {
  color: #17a2b8;
  text-decoration: none;
  padding: 0;
  font-size: 0.9rem;
}

.card .btn-link:hover {
  text-decoration: underline;
}

.card .btn-primary {
  background-color: #17a2b8;
  border-color: #17a2b8;
  font-size: 0.9rem;
}

.card .btn-primary:hover {
  background-color: #138496;
  border-color: #eba40a;
}

.card-img-top {
  width: 100%;
  height: auto; 
}
</style>
