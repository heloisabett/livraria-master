<script>
import LivrosApi from "@/api/livros.js";
const livrosApi = new LivrosApi();
export default {
  data() {
    return {
      livro: {},
      livros: [],
    };
  },
  async created() {
    this.livros = await livrosApi.buscarTodosOsLivros();
  },
  methods: {
    async add() {
      if (this.livro.id) {
        await livrosApi.atualizarLivro(this.livro);
      } else {
        await livrosApi.adicionarLivro(this.livro);
      }
      this.livros = await livrosApi.buscarTodosOsLivros();
      this.livro = {};
    },
    async excluir(livro) {
      await livrosApi.excluirLivro(livro.id);
      this.livros = await livrosApi.buscarTodosOsLivros();
    },
    edit(livro) {
      Object.assign(this.livro, livro);
    },
  },
};
</script>
<template>
  <div class="container">
    <div class="title">
      <h2>LIVROS</h2>
    </div>
    <div class="form_input">
      <input type="text" placeholder="Informe nome" v-model="livro.nome" @keydown.enter="add" />
      <input
        type="text"
        placeholder="Informe categoria"
        v-model="livro.categoria"
        @keydown.enter="add"
      />
      <input type="text" placeholder="Informe autor" v-model="livro.autor" @keydown.enter="add" />
      <input
        type="text"
        placeholder="Informe editora"
        v-model="livro.editora"
        @keydown.enter="add"
      />
      <button @click="add">Add</button>
    </div>
    <div class="list_livros">
      <table>
        <thead>
          <tr>
            <th>NOME</th>
            <th>CATEGORIA</th>
            <th>AUTOR</th>
            <th>EDITORA</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="livro in livros" :key="livro.id">
            <td>{{ livro.nome }}</td>
            <td>{{ livro.categoria }}</td>
            <td>{{ livro.autor }}</td>
            <td>{{ livro.editora }}</td>
            <td>
              <button @click="excluir(livro)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style>
header {
  border-radius: 0.8;
}

.title {
  text-align: center;
  margin: 2rem 0;
}

.form_input {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 2rem 0;
}

.form_input input {
  width: 50%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 10px;
  margin-bottom: 0.7%;
}

.form_input button {
  padding: 0.5rem;
  width: 15%;
  border: 1px solid rgb(112, 165, 233);
  border-radius: 10px;
  background-color: rgb(112, 165, 233);
  color: aliceblue;
  font-weight: bold;
  margin-left: 1%;
}

.list_times {
  display: flex;
  justify-content: center;
}

table {
  width: 85%;
  border-radius: 1px;
  margin: 0 auto;
  border: 1px solid rgb(103, 159, 207);
  font-size: 1.1rem;
  font-family: "Times New Roman", Times, serif;
  text-align: center;
}

table thead {
  background-color: rgb(103, 159, 207);
  color: white;
}

table tbody tr:nth-child(odd) {
  background-color: rgb(151, 189, 223);
  color: white;
}

h2 {
  font-family: "Times New Roman", Times, serif;
  font-size: 3em;
}

.button {
  font-family: "Times New Roman", Times, serif;
  font-size: 1em;
}

main {
  background-color: aliceblue;
}
</style>
