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

function selecionaMenu(indice) {
  indiceMenuSelecionado.value = indice
}

fetch('src/meusDados.json')
    .then(res => res.json())
    .then(data => sobreMim.value = data)

</script>

<template>
  <header>
    <div id="nav">
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <ul class='nav' >
          <li class='nav-item greetings' v-for='(menu, indice) in menus' :key="`itemMenu${indice}`">
            <a @click='() => selecionaMenu(indice)'> {{ menu }}</a>
          </li>
        </ul>
      </nav>
    </div>
  </header>
  <main id='app' style="width: 1300px">
    <h1>{{ tituloPagina }}</h1>
    <section v-show='indiceMenuSelecionado === 0'>
      <div class='wrapper'>
        <div >
          <p>{{ sobreMim.nome }}</p>
          <p>{{ sobreMim.descricao }}</p>
        </div>
      </div>
    </section>
    <section v-show='indiceMenuSelecionado === 1'>
      <div v-for='(dadoAcademico, indice) in sobreMim.dadosAcademicos' :key="`dadoAcademico${indice}`">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ dadoAcademico.instituicao }}</h5>
            <p class="card-text">{{ dadoAcademico.curso }}</p>
            <p class="card-text">{{ dadoAcademico.anoInicio }} - {{ dadoAcademico.anoTermino }}</p>
          </div>
        </div>
        <br/>
      </div>
    </section>
    <section v-show='indiceMenuSelecionado === 2'>
      <div v-for='(dadoProfissional, indice) in sobreMim.dadosProfissionais' :key="`dadoProfissional${indice}`">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ dadoProfissional.empresaOrgao }}</h5>
            <p class="card-text">{{ dadoProfissional.cargo }}</p>
            <p class="card-text">{{ dadoProfissional.periodo }}</p>
            <p class="card-text">{{ dadoProfissional.detalhes }}</p>
          </div>
        </div>
        <br/>
      </div>
    </section>
    <section v-show='indiceMenuSelecionado === 3'>
      <div v-for='(contato, indice) in sobreMim.contatos' :key="`contato${indice}`">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ contato.texto }}</h5>
            <p class="card-text">
              <a target='_blank' v-if="contato.tipo === 'link'" v-bind:href="contato.contato">{{contato.contato}}</a>
              <a target='_blank' v-else-if="contato.tipo === 'email'" v-bind:href="`mailTo:${contato.contato}`">{{contato.contato}}</a>
              <span v-else>{{ contato.contato}}</span>
            </p>
          </div>
        </div>
        <br/>
      </div>
    </section>
  </main>
</template>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
