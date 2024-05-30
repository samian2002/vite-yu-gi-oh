<template>
    <div class="results-count">
      <p>Total Results: {{ count }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ResultsCount',
    props: {
      count: {
        type: Number,
        required: true
      }
    }
  }
  </script>
  
  <style scoped>
  .results-count {
    margin: 20px 0;
    font-size: 1.2rem;
    color: #fff;
  }
  </style>
  