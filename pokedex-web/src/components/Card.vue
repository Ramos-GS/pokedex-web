<template>
  <div class="card" :style="{ background: bgColor }" @click="handleClick">
    <!-- Estiliza o fundo da carta dinamicamente com a cor recebida via prop 'bgColor' e adiciona evento de clique -->
    <img :src="image" alt="Imagem do Card" class="card-image" />
    <!-- Exibe a imagem do card de acordo com o valor da prop 'image' -->

    <div class="card-content">
      <h3 class="card-title">{{ title }}</h3>
      <!-- Exibe o título da carta passado pela prop 'title' -->
      
      <p class="card-description">{{ description }}</p>
      <!-- Exibe a descrição da carta passado pela prop 'description' -->

      <div class="card-types">
        <!-- Exibe os tipos do Pokémon, aplicando cor de fundo específica para cada tipo -->
        <span
          v-for="(type, index) in types"
          :key="index"
          :style="{ backgroundColor: getTypeColor(type) }"
          class="card-type"
        >
          {{ type }}
          <!-- Mostra o nome do tipo atual do Pokémon -->
        </span>
      </div>

      <div class="card-details">
        <!-- Exibe os detalhes do peso e altura do Pokémon -->
        <div class="card-detail">
          <i class="fas fa-weight"></i> 
          <!-- Ícone de peso -->
          <p class="card-weight">{{ weight }} kg</p>
          <!-- Exibe o peso recebido pela prop 'weight' -->
        </div>
        <div class="card-detail">
          <i class="fas fa-ruler-vertical"></i>
          <!-- Ícone de altura -->
          <p class="card-height">{{ height }} m</p>
          <!-- Exibe a altura recebida pela prop 'height' -->
        </div>
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
      // Os tipos do Pokémon são obrigatórios e devem ser passados como um array
    },
  },
  methods: {
    getTypeColor(type) {
      // Mapeamento das cores para cada tipo de Pokémon
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
    handleClick() {
      this.$emit('click');
      // Emite o evento de clique para o componente pai manipular
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
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.2);
  color: #333;
  transition: transform 0.3s, box-shadow 0.3s;
  /* Animação suave para efeitos de hover */
  margin: 15px auto;
  text-align: center;
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.8), rgba(230, 230, 230, 0.8)); 
  /* Fundo com gradiente suave */
}

.card:hover {
  transform: translateY(-5px) scale(1.02); 
  /* Eleva levemente o card e aplica um zoom ao passar o mouse */
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.3);
  /* Aumenta a intensidade da sombra no hover */
}

.card-image {
  width: 100%;
  height: 180px;
  object-fit: cover;
  border-bottom: 4px solid rgba(0, 0, 0, 0.1); 
}

.card-content {
  padding: 20px;
}

.card-title {
  font-size: 1.6em;
  margin: 0 0 8px;
  color: #2c3e50;
  font-family: 'Arial', sans-serif;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.7); 
}

.card-description {
  font-size: 1em;
  color: #7f8c8d;
  margin-bottom: 10px;

}

.card-types {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 10px;

}

.card-type {
  color: white;
  border-radius: 5px;
  padding: 5px 10px;
  margin-right: 5px;
  font-size: 0.9em;
 }

.card-details {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}

.card-detail {
  display: flex;
  align-items: center;
  margin: 0 10px;
}

.card-weight,
.card-height {
  font-size: 1.2em; 
  font-weight: bold; 
  color: #34495e; 
}

.card-weight i,
.card-height i {
  margin-right: 5px;
  color: #34495e;
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
    font-size: 1em;
  }
}
</style>
