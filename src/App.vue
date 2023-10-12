<script setup>
import { ref } from 'vue'

const carros = ref([
  { nome: "Fusca", valor: 1000, cor: "Azul" },
  { nome: "Variante", valor: 2000, cor: "Amarela" },
  { nome: "Brasilia", valor: 3000, cor: "Branca" }
])

const nomeCarro = ref('')
const valorCarro = ref('')
const corCarro = ref('')
const carroParaEdicao = ref(false) // auxiliar para função salvar

function adicionar() {
  carros.value.push({
    nome: nomeCarro.value,
    valor: valorCarro.value,
    cor: corCarro.value
  })

  limpar();
}

function editar(carro, carros) {
  let idx = -1

  carros.filter((c, i) => {
    if (c.nome == carro.nome) {
      return idx = i
    }
  })

  carros[idx].nome = nomeCarro.value
  carros[idx].valor = valorCarro.value
  carros[idx].cor = corCarro.value

  limpar();
}

function salvar() {
  if (carroParaEdicao.value)
    editar(carroParaEdicao.value, carros.value)
  else
    adicionar()
}

function limpar() {
  carroParaEdicao.value = ''
  nomeCarro.value = ''
  valorCarro.value = ''
  corCarro.value = ''
}

function preencherFormulario(carro) {
  carroParaEdicao.value = carro
  nomeCarro.value = carro.nome
  valorCarro.value = carro.valor
  corCarro.value = carro.cor
}

function excluir(carro, carros) {
  carros.splice(carros.indexOf(carro), 1)
}

function ordenarNome(carros, tipoOrdenacao) {
  let compararPorNome = (carroA, carroB) => {
    return carroA.nome.localeCompare(carroB.nome)
  }

  if (tipoOrdenacao == 'asc') {
    carros.sort(compararPorNome)
  }

  if (tipoOrdenacao == 'desc') {
    carros.sort(compararPorNome)
    carros.reverse()
  }
}

function ordenarCor(carros, tipoOrdenacao) {
  let compararPorCor = (carroA, carroB) => {
    return carroA.cor.localeCompare(carroB.cor)
  }

  if (tipoOrdenacao == 'asc') {
    carros.sort(compararPorCor)
  }

  if (tipoOrdenacao == 'desc') {
    carros.sort(compararPorCor)
    carros.reverse()
  }
}

function ordenarValor(carros, tipoOrdenacao) {
  let compararPorValor = (carroA, carroB) => {
    return carroA.valor - carroB.valor;
  }

  if (tipoOrdenacao == 'asc') {
    carros.sort(compararPorValor)
  }

  if (tipoOrdenacao == 'desc') {
    carros.sort(compararPorValor)
    carros.reverse()
  }
}

</script>

<template>
  <div>
    <h3>Carros</h3>
    <form class="form" @submit.prevent="salvar(carros)">
      <div class="nome">
        <label for="nome">Nome Carro: </label>
        <div>
          <input id="nome" type="text" v-model="nomeCarro" required />
        </div>
      </div>

      <div class="cor">
        <label for="cor">Cor Carro: </label>
        <div>
          <input id="cor" type="text" v-model="corCarro" required />
        </div>
      </div>

      <div class="valor">
        <label for="valor">Valor Carro: </label>
        <div>
          <input id="valor" type="text" v-model="valorCarro" required />
        </div>
      </div>

      <button type="submit">Salvar</button>
      <button @click="limpar()">Limpar</button>
    </form>
    <hr>

    <table v-if="carros.length > 0" class="tabelaCarros">


      <tr>
        <th width="120">
          Nome
          <a class="ordenar">
            <span @click="ordenarNome(carros, 'asc')">▼</span>
            <span @click="ordenarNome(carros, 'desc')">▲</span>
          </a>
        </th>
        <th width="120">
          Cor
          <a class="ordenar">
            <span @click="ordenarCor(carros, 'asc')">▼</span>
            <span @click="ordenarCor(carros, 'desc')">▲</span>
          </a>
        </th>
        <th width="120">Valor
          <a class="ordenar">
            <span @click="ordenarValor(carros, 'asc')">▼</span>
            <span @click="ordenarValor(carros, 'desc')">▲</span>
          </a>
          </th>
        <th>Opções</th>
      </tr>


      <tr v-for="carro in carros">

        <td>{{ carro.nome }}</td>
        <td>{{ carro.cor }}</td>
        <td>{{ carro.valor }}</td>
        <td>
          <a @click="excluir(carro, carros)">Excluir</a>
          <a @click="preencherFormulario(carro, carros)">Editar</a>
        </td>
      </tr>

    </table>

    <div v-if="carros.length == 0">
      Não há carros para exibir
    </div>
  </div>
</template>