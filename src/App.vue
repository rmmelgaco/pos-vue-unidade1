<script setup>
import {ref, reactive, watch, computed} from 'vue'

const sobreMim = ref({})
let indiceMenuSelecionado = ref(0)

let menus = [
  'Sobre mim',
  'Acadêmico',
  'Profissional',
  'Contatos'
]

let mensagens = reactive([])
let novaMensagem = ref(null)
let chegouNovaMensagem = ref(false)
let erro = ref(null)

const tituloPagina = computed(() => menus[indiceMenuSelecionado.value])

function selecionaMenu(indice) {
  indiceMenuSelecionado.value = indice
}

function enviaMensagem() {
  const textoMensagem = novaMensagem.value
  if (textoMensagem) {
    const dataMensagem = new Date().toLocaleString('pt-BR')
    mensagens.push({
      textoMensagem,
      dataMensagem
    })
    novaMensagem.value = null
  } else {
    erro.value = 'Digite uma mensagem antes de enviar.'
  }
}

function fechaMensagemAlerta() {
  chegouNovaMensagem.value = false
}

function fechaMensagemErro() {
  erro.value = false
}

watch(() => mensagens.length, (totalMensagensDepois, totalMensagensAntes) => {
  if (totalMensagensDepois > totalMensagensAntes) {
    chegouNovaMensagem.value = true
  }
})

fetch('src/meusDados.json')
    .then(res => res.json())
    .then(data => sobreMim.value = data)

</script>

<template>
  <header>
    <div id="nav">
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <ul class='nav'>
          <li class='nav-item greetings' v-for='(menu, indice) in menus' :key="`itemMenu${indice}`">
            <a @click='() => selecionaMenu(indice)'> {{ menu }}</a>
          </li>
        </ul>
      </nav>
    </div>
  </header>
  <main id='app' style="width: 1300px">
    <h1>{{ tituloPagina }}</h1>
    <div v-if='chegouNovaMensagem' class="alert alert-warning" role="alert">
      Chegou uma nova mensagem no chat.
      <a @click='fechaMensagemAlerta'>X</a>
    </div>
    <div v-if='erro' class="alert alert-danger" role="alert">
      {{ erro }}
      <a @click='fechaMensagemErro'>X</a>
    </div>
    <section v-show='indiceMenuSelecionado === 0'>
      <div class='wrapper'>
        <div>
          <h3>{{ sobreMim.nome }}</h3>
          <br />
          <h6>{{ sobreMim.descricao }}</h6>
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
              <a target='_blank' v-if="contato.tipo === 'link'" v-bind:href="contato.contato">{{ contato.contato }}</a>
              <a target='_blank' v-else-if="contato.tipo === 'email'"
                 v-bind:href="`mailTo:${contato.contato}`">{{ contato.contato }}</a>
              <span v-else>{{ contato.contato }}</span>
            </p>
          </div>
        </div>
        <br/>
      </div>
    </section>
  </main>
  <footer>
    <div class='card'>
      <h4 class='tituloMensagemChat'>Mensagens</h4>
      <p v-if='!mensagens.length'>Nenhuma mensagem recebida.</p>
      <span v-for='(mensagem, indice) in mensagens' :key="`mensagem${indice}`">
      <p>{{ mensagem.textoMensagem }} - {{ mensagem.dataMensagem }}</p>
    </span>
      <div>
        <textarea class="form-control" v-model='novaMensagem'/>
        <button type='button' class="btn btn-primary" @click='enviaMensagem'>Enviar</button>
      </div>
    </div>
  </footer>
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
