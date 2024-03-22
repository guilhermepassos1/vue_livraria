<script setup>
import { ref, reactive, onMounted } from "vue";
import AutoresApi from "@/api/autores";
const AutoresApi = new AutoresApi();

const defaultAutores = { id: null, descricao: "" };
const autores = ref([]);
const autores = reactive({ ...defaultAutor });

onMounted(async () => {
  autores.value = await autoresApi.buscarTodasAsAutores();
});

function limpar() {
  Object.assign(autor, { ...defaultAutor });
}

async function salvar() {
  if (autor.id) {
    await autoresApi.atualizarAutor(autor);
  } else {
    await autoresApi.adicionarAutor(autor);
  }
  autores.value = await autoresApi.buscarTodasAsAutores();
  limpar();
}

function editar(autor_para_editar) {
  Object.assign(autor, autor_para_editar);
}

async function excluir(id) {
  await autoresApi.excluirAutor(id);
  autores.value = await autoresApi.buscarTodasAsAutores();
  limpar();
}
</script>

<template>
  <h1>Autor</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="autor.descricao" placeholder="Descrição" />
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
    <li v-for="autor in autores" :key="autor.id">
      <span @click="editar(autor)">
        ({{ autor.id }}) - {{ autor.descricao }} -
      </span>
      <button @click="excluir(autor.id)">X</button>
    </li>
  </ul>
</template>

<style></style>
