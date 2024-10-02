<template>
  <div class="modal" v-if="isOpen">
    <div class="modal-overlay" @click="closeModal"></div>
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>

      <!-- Imagem do Pokémon -->
      <img :src="pokemon.image" alt="Imagem do Pokémon" class="pokemon-image" />

      <!-- Número e Nome -->
      <h3>#{{ pokemon.pokedexNumber }} - {{ pokemon.name }}</h3>

      <!-- Tipos -->
      <div class="pokemon-types">
        <span
          v-for="(type, index) in pokemon.types"
          :key="index"
          :style="{ backgroundColor: getTypeColor(type) }"
          class="pokemon-type"
        >
          {{ type }}
        </span>
      </div>

      <!-- Medidas: Altura e Peso -->
      <div class="pokemon-measurements">
        <div class="measurements-container">
          <p><strong>Altura:</strong> {{ pokemon.height }} m</p>
          <p><strong>Peso:</strong> {{ pokemon.weight }} kg</p>
        </div>
      </div>

      <!-- Fraquezas -->
      <div class="pokemon-weaknesses">
        <h4>Fraquezas</h4>
        <div>
          <span
            v-for="(weakness, index) in weaknesses"
            :key="index"
            :style="{ backgroundColor: getTypeColor(weakness) }"
            class="pokemon-weakness"
          >
            {{ weakness }}
          </span>
        </div>
      </div>

      <!-- Estatísticas -->
      <div class="pokemon-stats">
        <h4>Estatísticas</h4>
        <ul>
          <li v-for="(statValue, statName) in stats" :key="statName">
            <span class="stat-name">{{ statName }}:</span>
            <div class="stat-bar">
              <div
                class="stat-fill"
                :style="{ width: statValue + '%' }"
              ></div>
              <span class="stat-value">{{ statValue }}</span>
            </div>
          </li>
        </ul>
      </div>

      <!-- Evoluções -->
      <div class="pokemon-evolution" v-if="evolutionChain.length">
        <h4>Evoluções</h4>
        <div class="evolution-chain">
          <div v-for="(evolution, index) in evolutionChain" :key="evolution.name" class="evolution-item">
            <img :src="evolution.image" :alt="evolution.name" class="evolution-image" />
            <div class="evolution-level" v-if="evolution.level">
              Lvl {{ evolution.level }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Modal',
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
    pokemon: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      stats: {},
      weaknesses: [],
      evolutionChain: [],
    };
  },
  async created() {
    await this.fetchPokemonDetails();
  },
  methods: {
    async fetchPokemonDetails() {
      try {
        const detailResponse = await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.pokemon.id}`);
        this.stats = {
          Hp: detailResponse.data.stats[0].base_stat,
          Attack: detailResponse.data.stats[1].base_stat,
          Defense: detailResponse.data.stats[2].base_stat,
          SpecialAttack: detailResponse.data.stats[3].base_stat,
          SpecialDefense: detailResponse.data.stats[4].base_stat,
          Speed: detailResponse.data.stats[5].base_stat,
        };

        const speciesResponse = await axios.get(`https://pokeapi.co/api/v2/pokemon-species/${this.pokemon.id}`);
        
        const evolutionUrl = speciesResponse.data.evolution_chain.url;
        const evolutionResponse = await axios.get(evolutionUrl);
        this.evolutionChain = this.formatEvolutionChain(evolutionResponse.data.chain);

        this.weaknesses = await this.fetchWeaknesses(this.pokemon.types);
      } catch (error) {
        console.error('Erro ao buscar detalhes do Pokémon:', error);
      }
    },
    formatEvolutionChain(chain) {
      const evolutions = [];
      let current = chain;

      while (current) {
        const evolutionId = this.extractIdFromUrl(current.species.url);
        evolutions.push({
          name: current.species.name,
          image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${evolutionId}.png`,
          level: current.evolves_to.length > 0 ? current.evolves_to[0].evolution_details[0]?.min_level : null,
        });
        current = current.evolves_to[0];
      }

      return evolutions;
    },
    async fetchWeaknesses(types) {
      const weaknesses = new Set();
      for (const type of types) {
        const response = await axios.get(`https://pokeapi.co/api/v2/type/${type}`);
        response.data.damage_relations.double_damage_from.forEach(weak => weaknesses.add(weak.name));
      }
      return Array.from(weaknesses);
    },
    extractIdFromUrl(url) {
      const segments = url.split('/');
      return segments[segments.length - 2];
    },
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
    closeModal() {
      this.$emit('close');
    },
  },
};
</script>

<style scoped>
.measurements-container {
  display: flex;
  justify-content: space-around;
  margin: 10px 0;
}

.measurements-container p {
  margin: 0;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
  backdrop-filter: blur(5px);
}

.modal-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
}

.modal-content {
  background: linear-gradient(135deg, #a0c5fc, #bcc3ce);
  padding: 20px;
  border-radius: 15px;
  position: relative;
  z-index: 10;
  width: 90%; 
  max-width: 600px; 
  text-align: center;
  overflow-y: auto; 
  max-height: 90%;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  transition: all 0.3s ease-in-out;
  color: black; 
}

.pokemon-image {
  width: 100%;
  max-width: 220px;
  height: auto;
  margin: 0 auto;
  border-radius: 10px;
}

.close {
  position: absolute;
  top: 10px;
  right: 15px;
  font-size: 24px;
  cursor: pointer;
  color: #f12610;
  transition: color 0.2s;
}

.close:hover {
  color: #ad1706;
}

.pokemon-types,
.pokemon-weaknesses {
  margin: 10px 0;
}

.pokemon-type,
.pokemon-weakness {
  display: inline-block;
  padding: 8px 12px;
  border-radius: 20px;
  color: rgb(0, 0, 0); 
  margin: 5px;
  font-weight: bold;
  transition: transform 0.2s;
}

.pokemon-type:hover,
.pokemon-weakness:hover {
  transform: scale(1.1);
}

.pokemon-measurements {
  margin: 10px 0;
}

.pokemon-stats {
  margin: 10px 0;
  text-align: left;
}

.pokemon-stats ul {
  list-style: none;
  padding: 0;
}

.pokemon-stats li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 8px;
}

.stat-bar {
  width: 60%;
  height: 12px;
  background-color: #ccc;
  border-radius: 5px;
  overflow: hidden;
  position: relative;
}

.stat-fill {
  height: 100%;
  background-color: #3498db;
  border-radius: 5px;
}

.stat-name {
  width: 40%;
}

.stat-value {
  position: absolute;
  right: 10px;
  font-size: 10px;
}

.evolution-chain {
  display: flex;
  justify-content: center;
  margin: 15px 0;
}

.evolution-item {
  margin: 0 10px;
  text-align: center;
}

.evolution-image {
  width: 60px;
  height: 60px;
}

.evolution-level {
  font-weight: bold;
  margin-top: 5px;
}

@media (max-width: 768px) {
  .modal-content {
    width: 95%; 
    padding: 15px;
  }

  .pokemon-image {
    max-width: 180px;
  }
}
</style>
