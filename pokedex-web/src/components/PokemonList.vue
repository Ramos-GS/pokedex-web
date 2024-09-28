<template>
    <div class="pokemon-list">
      <Card
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        :title="pokemon.name"
        :description="`Num: 0${pokemon.pokedexNumber} - Tipo: ${pokemon.types.join(', ')}`"
        :image="pokemon.image"
        :weight="pokemon.weight"
        :height="pokemon.height"
        :bgColor="getColorByType(pokemon.types[0])" 
      />
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import Card from './Card.vue';
  
  export default {
    name: 'PokemonList',
    components: {
      Card,
    },
    data() {
      return {
        pokemons: [],
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
      };
    },
    async created() {
      await this.fetchPokemons();
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
        } catch (error) {
          console.error('Erro ao buscar Pokémon:', error);
        }
      },
      getColorByType(type) {
        return this.typeColors[type] || '#FFFFFF'; 
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
  