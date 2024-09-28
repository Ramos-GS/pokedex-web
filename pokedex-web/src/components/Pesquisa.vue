<template>
  <div>
    <input
      type="text"
      v-model="searchQuery"
      placeholder="Pesquisar seu pokemon"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const searchQuery = ref<string>('');
</script>

<style scoped>
input {
  margin-bottom: 250px;
  padding: 10px;
  border: 1px solid #ffffff;
  border-radius: 10px;
  width: 600px;
  font-size: 16px;
  box-shadow: 0 4px 8px rgba(253, 253, 253, 0.2);
}
</style>
