<template>
  <div class="pokemon-list">
    <!-- Componente de filtro de busca e tipos -->
    <SearchFilter @search="searchPokemons" @filter="filterPokemons" />
    
    <!-- Loop pelos Pokémon filtrados, exibindo o componente Card para cada um -->
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

    <!-- Componente de Modal, exibido apenas se um Pokémon for selecionado -->
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
      pokemons: [], // Armazena a lista de todos os Pokémon
      filteredPokemons: [], // Armazena os Pokémon filtrados
      selectedPokemon: null, // Pokémon atualmente selecionado
      typeColors: { // Cores associadas a cada tipo de Pokémon
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
      searchQuery: '', // Consulta de pesquisa (busca)
      selectedTypes: [], // Tipos de Pokémon selecionados para filtragem
    };
  },
  async created() {
    // Busca os dados dos Pokémon ao criar o componente
    await this.fetchPokemons();
    this.filteredPokemons = this.pokemons; // Inicializa a lista filtrada com todos os Pokémon
  },
  methods: {
    // Função para buscar Pokémon da PokeAPI
    async fetchPokemons() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=151');
        // Para cada Pokémon, busca os detalhes adicionais
        this.pokemons = await Promise.all(
          response.data.results.map(async (pokemon) => {
            const detailResponse = await axios.get(pokemon.url);
            return {
              id: detailResponse.data.id,
              pokedexNumber: detailResponse.data.id,
              name: detailResponse.data.name,
              image: detailResponse.data.sprites.front_default,
              weight: (detailResponse.data.weight / 10).toFixed(1), // Converte peso de decigramas para kg
              height: (detailResponse.data.height / 10).toFixed(1), // Converte altura de decímetros para metros
              types: detailResponse.data.types.map(type => type.type.name), // Extrai os tipos do Pokémon
            };
          })
        );
        // Atualiza a lista de Pokémon filtrada após a busca inicial
        this.filteredPokemons = this.pokemons;
      } catch (error) {
        console.error('Erro ao buscar Pokémon:', error);
      }
    },

    // Função que retorna a cor de fundo baseada no tipo de Pokémon
    getColorByType(type) {
      return this.typeColors[type] || '#FFFFFF'; // Retorna cor padrão se o tipo não for encontrado
    },

    // Define o Pokémon selecionado ao clicar no Card
    selectPokemon(pokemon) {
      this.selectedPokemon = pokemon;  
    },

    // Fecha o modal do Pokémon
    closeModal() {
      this.selectedPokemon = null;  
    },

    // Realiza busca com base na consulta inserida
    searchPokemons(query) {
      this.searchQuery = query.toLowerCase();
      this.applyFilters(); // Aplica a filtragem com base na consulta e nos tipos
    },

    // Filtra Pokémon pelos tipos selecionados
    filterPokemons(types) {
      this.selectedTypes = types;
      this.applyFilters(); // Aplica a filtragem com base na consulta e nos tipos
    },

    // Aplica a filtragem tanto para a busca quanto para os tipos selecionados
    applyFilters() {
      this.filteredPokemons = this.pokemons.filter(pokemon => {
        const matchesSearch = pokemon.name.toLowerCase().includes(this.searchQuery) || 
                              pokemon.pokedexNumber.toString().includes(this.searchQuery);
        const matchesType = this.selectedTypes.length === 0 || 
                            this.selectedTypes.some(type => pokemon.types.includes(type));
        return matchesSearch && matchesType; // Retorna apenas os Pokémon que correspondem à busca e ao tipo
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
