<template>
  <div>
    <titulo texto="Aluno"></titulo>
    <input type="text" placeholder="Nome do Aluno" v-model="nome" @keyup.enter="addAluno()">
    <button class="btn btnInput" @click="addAluno()">Adicionar</button>
    <!--table>(thead>th*3)+tbody>tr>td*3-->
    <table>
      <thead>
        <th>Mat.</th>
        <th>Nome</th>
        <th>Opções</th>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="(aluno, index) in alunos" :key="index">
          <td>{{ aluno.id }}</td>
          <td>{{ aluno.nome }}</td>
          <td>
            <Button class="btn btn_Danger" @click="remover(aluno)">Remover</Button>
          </td>
        </tr>
      </tbody>
      <tfoot v-else>Nenhum Aluno Encontrado</tfoot>
    </table>
  </div>
</template>

<script>
import Titulo from "../_share/Titulo";

export default {
  components: {
    Titulo
  },
  data() {
    return {
      titulo: "Aluno",
      nome: "",
      alunos: []
    };
  },
  created() {
    this.$http
      .get("http://localhost:3000/alunos")
      .then(res => res.json())
      .then(alunos => (this.alunos = alunos));
  },
  props: {},
  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome: ""
      };

      this.$http
        .post("http://localhost:3000/alunos", _aluno)
        .then(res => res.json())
        .then(alunoRetornado => {
          this.alunos.push(alunoRetornado);
          this.nome = "";
        });
    },
    remover(aluno) {
      this.$http.delete(`http://localhost:3000/alunos/${aluno.id}`).then(() => {
        let indice = this.alunos.indexOf(aluno);
        this.alunos.splice(indice, 1);
      });
    }
  }
};
</script>

<!--style scoped-->
<style scoped>
input {
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
}
.btnInput {
  border: 0px;
  padding: 20px;
  font-size: 1.3em;
  background-color: rgb(116, 115, 115);
  display: inline;
}
.btnInput:houver {
  padding: 20px;
  margin: 0px;
  border: 0px;
}
</style>
