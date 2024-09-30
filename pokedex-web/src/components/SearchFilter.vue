<template>
  <div class="search-filter">
    <div class="search-input-wrapper">
      <input
        type="text"
        v-model="searchQuery"
        placeholder="Buscar Pokémon por nome ou número"
        @input="emitSearch"
        class="search-input"
      />
      <span class="search-icon">🔍</span>
    </div>
    <div class="type-filters">
      <button
        v-for="(type, index) in allTypes"
        :key="index"
        :class="['type-button', { selected: selectedTypes.includes(type) }]"
        @click="toggleType(type)"
        class="type-button"
      >
        {{ type }}
      </button>
    </div>
  </div>
</template>



<script>
export default {
  name: "SearchFilter",
  data() {
    return {
      searchQuery: "",
      selectedTypes: [],
      allTypes: [
        "fire", "water", "grass", "electric", "ice",
        "fighting", "poison", "ground", "flying",
        "psychic", "bug", "rock", "ghost", "dragon",
        "dark", "steel", "fairy", "normal"
      ],
    };
  },
  methods: {
    emitSearch() {
      this.$emit("search", this.searchQuery);
    },
    toggleType(type) {
      if (this.selectedTypes.includes(type)) {
        this.selectedTypes = this.selectedTypes.filter((t) => t !== type);
      } else {
        this.selectedTypes.push(type);
      }
      this.emitFilter();
    },
    emitFilter() {
      this.$emit("filter", this.selectedTypes);
    },
  },
};
</script>


<style scoped>
.search-filter {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
  padding: 20px;
}

.search-input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.search-input {
  width: 100%;
  padding: 12px 20px 12px 40px; /* Espaço para o ícone de busca */
  border: none;
  border-radius: 30px;
  background-color: #f1f3f4;
  font-size: 16px;
  color: #333;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.search-input::placeholder {
  color: #888;
}

.search-input:focus {
  background-color: #fff;
  box-shadow: 0 0 8px rgba(0, 123, 255, 0.4);
  outline: none;
}

.search-icon {
  position: absolute;
  left: 10px;
  font-size: 18px;
  color: #888;
}

.type-filters {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.type-button {
  padding: 10px 20px;
  border: none;
  border-radius: 20px;
  background-color: #f1f3f4;
  font-size: 14px;
  color: #333;
  text-transform: capitalize;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.type-button.selected {
  background-color: #4caf50;
  color: #fff;
  box-shadow: 0 0 8px rgba(0, 123, 255, 0.3);
}

.type-button:hover {
  background-color: #e0e0e0;
  box-shadow: 0 0 6px rgba(0, 0, 0, 0.1);
}
</style>
