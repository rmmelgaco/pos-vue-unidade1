<script setup>
import {ref} from 'vue';

const titulo = ref('Teste rodrigo');

function alteraTitulo() {
  titulo.value += 1;
}

</script>

<template>
  <main>
    <h2>{{ titulo }}</h2>
    <button @click='alteraTitulo'>
      Altera título
    </button>
  </main>
</template>

<style scoped>
</style>
