<template>
    <div class="card">
      <img :src="image" alt="Imagem do Card" class="card-image" />
      <div class="card-content">
        <h3 class="card-title">{{ title }}</h3>
        <p class="card-description">{{ description }}</p>
        <div class="card-details">
          <p class="card-weight">Peso: {{ weight }} kg</p>
          <p class="card-height">Altura: {{ height }} m</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Card',
    props: {
      title: {
        type: String,
        required: true,
      },
      description: {
        type: String,
        required: true,
      },
      image: {
        type: String,
        required: true,
      },
      weight: {
        type: Number,
        required: true,
      },
      height: {
        type: Number,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .card {
    border: 1px solid #e0e0e0;
    border-radius: 12px;
    overflow: hidden;
    width: 240px; 
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
    color: #333;
    transition: transform 0.2s;
  }
  
  .card:hover {
    transform: scale(1.02);
  }
  
  .card-image {
    width: 100%;
    height: 150px; 
    object-fit: cover; 
  }
  
  .card-content {
    padding: 16px; 
  }
  
  .card-title {
    font-size: 1.4em; 
    margin: 0 0 8px;
    color: #2c3e50;
  }
  
  .card-description {
    font-size: 0.9em; 
    color: #7f8c8d;
    margin-bottom: 10px; 
  }
  
  .card-details {
    display: flex;
    justify-content: space-between; 
    font-size: 0.8em; 
    color: #95a5a6;
  }
  </style>
  