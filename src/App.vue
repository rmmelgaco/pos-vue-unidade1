<script setup>
import {ref, computed} from 'vue'

const sobreMim = ref({})
let indiceMenuSelecionado = ref(0)

let menus = [
  'Sobre mim',
  'Acadêmico',
  'Profissional',
  'Contatos'
]

const tituloPagina = computed(() => menus[indiceMenuSelecionado.value])

function selecionaMenu (indice) {
  indiceMenuSelecionado.value = indice
}

fetch('src/meusDados.json')
    .then(res => res.json())
    .then(data => sobreMim.value = data)

</script>

<template>
  <header>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <ul class='nav'>
      <li class='nav-item' v-for='(menu, indice) in menus'>
        <a @click='() => selecionaMenu(indice)'> {{ menu }}</a>
      </li>
    </ul>
    </nav>
  </header>
  <main>
    <h1>{{ tituloPagina }}</h1>
    <h2>{{ sobreMim.nome }}</h2>
  </main>
</template>

<style scoped>
</style>
