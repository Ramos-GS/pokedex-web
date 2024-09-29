<template>
  <div class="card" :style="{ backgroundColor: bgColor }">
    <img :src="image" alt="Imagem do Card" class="card-image" />
    <div class="card-content">
      <h3 class="card-title">{{ title }}</h3>
      <p class="card-description">{{ description }}</p>
      <div class="card-types">
        <span v-for="(type, index) in types" :key="index" :style="{ backgroundColor: getTypeColor(type) }" class="card-type">{{ type }}</span>
      </div>
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
      required: true,
    },
    types: {
      type: Array,
      required: true,
    },
  },
  methods: {
    getTypeColor(type) {
      const typeColors = {
        fire: '#FF5733', 
        water: '#3498DB', 
        grass: '#2ECC71', 
        electric: '#F1C40F', 
        ice: '#5DADE2', 
        fighting: '#E74C3C', 
        poison: '#9B59B6', 
        ground: '#D4AC0D',
        flying: '#5DADE2', 
        psychic: '#E91E63', 
        bug: '#7DCEA0', 
        rock: '#A04000',
        ghost: '#8E44AD', 
        dragon: '#8E44AD', 
        dark: '#34495E', 
        steel: '#BDC3C7', 
        fairy: '#F5B7B1', 
        normal: '#BDC3C7', 
      };
      return typeColors[type] || '#BDC3C7'; 
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

.card-types {
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 10px;
}

.card-type {
  color: white; /* Texto branco para maior contraste */
  border-radius: 5px;
  padding: 5px 10px;
  margin-right: 5px;
  font-size: 0.9em;
}

.card-details {
  display: flex;
  justify-content: space-between;
  font-size: 0.9em;
  color: #95a5a6;
  margin-bottom: 10px;
}

/* Media queries para responsividade */
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
