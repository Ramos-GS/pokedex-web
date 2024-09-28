<template>
    <div class="card" role="article" aria-labelledby="card-title" :style="{ backgroundColor: bgColor }">
      <img :src="image" alt="Imagem do {{ title }}" class="card-image" />
      <div class="card-content">
        <h3 id="card-title" class="card-title">{{ title }}</h3>
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
      bgColor: {
        type: String,
        required: false,
        default: '#ffffff',
      },
    },
  };
  </script>
  
  <style scoped>
  .card {
    border-radius: 15px; 
    overflow: hidden;
    width: 90%; 
    max-width: 300px; 
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
    color: #333;
    transition: transform 0.2s, box-shadow 0.2s;
    margin: 15px auto; 
  }
  
  .card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.15);
  }
  
  .card-image {
    width: 100%;
    height: 180px;
    object-fit: cover; 
  }
  
  .card-content {
    padding: 20px; 
  }
  
  .card-title {
    font-size: 1.5em;
    margin: 0 0 8px; 
    color: #2c3e50;
    font-family: 'Arial', sans-serif; 
  }
  
  .card-description {
    font-size: 1em;
    color: #7f8c8d;
    margin-bottom: 10px; 
  }
  
  .card-details {
    display: flex;
    justify-content: space-between; 
    font-size: 0.9em; 
    color: #95a5a6;
    margin-bottom: 10px; 
  }
  
  @media (max-width: 600px) {
    .card {
      width: 95%; 
      margin: 10px auto; 
    }
  
    .card-title {
      font-size: 1.3em; 
    }
  
    .card-description {
      font-size: 0.9em; 
    }
  
    .card-details {
      font-size: 0.8em; 
    }
  }
  </style>
  