<script setup>
import {ref} from 'vue'

const sobreMim = ref({})

fetch('src/meusDados.json')
    .then( res => res.json())
    .then(data => sobreMim.value = data)

</script>

<template>
  <main>
    <h2>{{ sobreMim.nome }}</h2>
  </main>
</template>

<style scoped>
</style>
