<template>
    <div class="pokemon-list">
      <Card
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        :title="pokemon.name"
        :description="`Pokémon ID: ${pokemon.id}`"
        :image="pokemon.image"
        :weight="pokemon.weight"
        :height="pokemon.height"
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
                name: detailResponse.data.name,
                image: detailResponse.data.sprites.front_default,
                weight: (detailResponse.data.weight / 10).toFixed(1), 
                height: (detailResponse.data.height / 10).toFixed(1), 
              };
            })
          );
        } catch (error) {
          console.error('Erro ao buscar Pokémon:', error);
        }
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
  