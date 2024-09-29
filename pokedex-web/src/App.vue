<template>
  <div id="app">
    <h1>Pokédex</h1>
    <PokemonList />
  </div>
</template>

<script>
import PokemonList from './components/PokemonList.vue';

export default {
  name: 'App',
  components: {
    PokemonList,
  },
};
</script>

<style>
h1 {
  margin-bottom: 200px;
}
</style>
