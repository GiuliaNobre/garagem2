<script setup>
import { ref, reactive, onMounted } from "vue";
import VeiculosApi from "@/api/veiculos";
const veiculosApi = new VeiculosApi();

const defaultVeiculo = { id: null, modelo: "", cor: "", ano: "", preco: "", acessorios: "", };
const veiculos = ref([]);
const veiculo = reactive({ ...defaultVeiculo });

onMounted(async () => {
  veiculos.value = await veiculosApi.buscarTodosOsVeiculos();
});

function limpar() {
  Object.assign(veiculo, { ...defaultVeiculo });
}

async function salvar() {
  if (veiculo.id) {
    await veiculosApi.atualizarVeiculo(veiculo);
  } else {
    await veiculosApi.adicionarVeiculo(veiculo);
  }
  veiculos.value = await veiculosApi.buscarTodosOsVeiculos();
  limpar();
}

function editar(veiculo_para_editar) {
  Object.assign(veiculo, veiculo_para_editar);
}

async function excluir(id) {
  await veiculosApi.excluirVeiculo(id);
  veiculos.value = await veiculosApi.buscarTodosOsVeiculos();
  limpar();
}
</script>

<template>
  <h1>Veiculo</h1>
  <hr />
  <div class="form">
    <select v-model="veiculo.modelo"  >
      <option value=""></option> 
    </select>
    <select v-model="veiculo.cor"  >
      <option value=""></option> 
    </select>
    <input type="text" v-model="veiculo.ano" placeholder="ano" />
    <input type="text" v-model="veiculo.preco" placeholder="preço" />
    
    <select v-model="veiculo.acessorios"  >
      <option value=""></option> 
    </select>
    
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
    <li v-for="veiculo in veiculos" :key="veiculo.id">
      <span @click="editar(veiculo)">
        ({{ veiculo.id }}) - {{ veiculo.modelo }} - {{ veiculo.ano}} - {{ veiculo.cor }}
      </span>
      <button @click="excluir(veiculo.id)">X</button>
    </li>
  </ul>
</template>

<style></style>