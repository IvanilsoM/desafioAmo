<template>
  <div class="AppDesafio">
    <ToDoDesafio v-bind:tarefas="listaDeTarefas" @adicionar-tarefa="adicionarTarefa" @excluir-tarefa="excluirTarefa" @excluir-tarefas-concluidas="excluirTarefasConcluidas"></ToDoDesafio> <!--/caso não passe o @adicionar-tarefa="adicionarTarefa" ou o @excluir-tarefa="excluirTarefa" o componente filho não conseguirá acessar e emitir as informações ao componente pai, ou seja, pode excluir ou adicionar, mas no componente pai o array será sempre igual-->
    <!-- <button @click="addTarefa">Adicionar Tarefas</button> -->
  </div>
</template>

<script>

import ToDoDesafio from "./components/ToDoDesafio.vue";
import '@fortawesome/fontawesome-free/css/all.css';


export default {
  name: "AppDesafio",
  components: {
    ToDoDesafio,
  },
  data() {
    return {
      listaDeTarefas: [
        { id: 1, nome: "fazer compras", completa: true },
        { id: 2, nome: "Estudar", completa: false },
        { id: 3, nome: "Capinar", completa: true },
        { id: 4, nome: "Trabalhar", completa: false },
      ],
    };
  },
  methods: {
      adicionarTarefa(novaTarefa){
      this.listaDeTarefas.push(novaTarefa);
    },
    excluirTarefa(id) {
      const indice = this.listaDeTarefas.findIndex((tarefa) => tarefa.id === id);
      if (indice !== -1) {
        this.listaDeTarefas.splice(indice, 1);
      }
    },
    excluirTarefasConcluidas() {
      this.listaDeTarefas = this.listaDeTarefas.filter((tarefa) => !tarefa.completa);
    },
  },
};
</script>
