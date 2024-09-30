<template>
  <div class="pokemon-list">
    <SearchFilter @search="searchPokemons" @filter="filterPokemons" />
    <Card
      v-for="pokemon in filteredPokemons"
      :key="pokemon.id"
      :title="pokemon.name"
      :description="`Num: 0${pokemon.pokedexNumber}`"
      :image="pokemon.image"
      :weight="pokemon.weight"
      :height="pokemon.height"
      :bgColor="getColorByType(pokemon.types[0])"
      :types="pokemon.types"
      @click.native="selectPokemon(pokemon)" 
    />
    <Modal v-if="selectedPokemon" :isOpen="!!selectedPokemon" :pokemon="selectedPokemon" @close="closeModal" />
  </div>
</template>

<script>
import axios from 'axios';
import Card from './Card.vue';
import Modal from './Modal.vue'; 
import SearchFilter from './SearchFilter.vue';

export default {
  name: 'PokemonList',
  components: {
    Card,
    Modal, 
    SearchFilter,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      selectedPokemon: null,
      typeColors: {
        fire: '#FFCCBC',
        water: '#BBDEFB',
        grass: '#C8E6C9',
        electric: '#FFF9C4',
        ice: '#E1F5FE',
        fighting: '#FFAB91',
        poison: '#D1C4E9',
        ground: '#FFE0B2',
        flying: '#B3E5FC',
        psychic: '#F8BBD0',
        bug: '#C5E1A5',
        rock: '#D7CCC8',
        ghost: '#E0E0E0',
        dragon: '#B39DDB',
        dark: '#B0BEC5',
        steel: '#B0BEC5',
        fairy: '#F1BEE7',
        normal: '#FFFFFF',
      },
      searchQuery: '',
      selectedTypes: [],
    };
  },
  async created() {
    await this.fetchPokemons();
    this.filteredPokemons = this.pokemons; // Inicializa a lista filtrada
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=151');
        this.pokemons = await Promise.all(
          response.data.results.map(async (pokemon) => {
            const detailResponse = await axios.get(pokemon.url);
            return {
              id: detailResponse.data.id,
              pokedexNumber: detailResponse.data.id,
              name: detailResponse.data.name,
              image: detailResponse.data.sprites.front_default,
              weight: (detailResponse.data.weight / 10).toFixed(1),
              height: (detailResponse.data.height / 10).toFixed(1),
              types: detailResponse.data.types.map(type => type.type.name),
            };
          })
        );
        this.filteredPokemons = this.pokemons; // Atualiza a lista filtrada após buscar os Pokémon
      } catch (error) {
        console.error('Erro ao buscar Pokémon:', error);
      }
    },
    getColorByType(type) {
      return this.typeColors[type] || '#FFFFFF'; 
    },
    selectPokemon(pokemon) {
      this.selectedPokemon = pokemon;  
    },
    closeModal() {
      this.selectedPokemon = null;  
    },
    searchPokemons(query) {
      this.searchQuery = query.toLowerCase();
      this.applyFilters(); // Atualiza a filtragem após a busca
    },
    filterPokemons(types) {
      this.selectedTypes = types;
      this.applyFilters(); // Atualiza a filtragem após a seleção de tipos
    },
    applyFilters() {
      this.filteredPokemons = this.pokemons.filter(pokemon => {
        const matchesSearch = pokemon.name.toLowerCase().includes(this.searchQuery) || 
                              pokemon.pokedexNumber.toString().includes(this.searchQuery);
        const matchesType = this.selectedTypes.length === 0 || 
                            this.selectedTypes.some(type => pokemon.types.includes(type));
        return matchesSearch && matchesType; // Filtra com base na busca e no tipo
      });
    },
  },
};
</script>

<style scoped>
.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
