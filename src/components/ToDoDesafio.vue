<template>
  <div class="container">
    <div class="tarefaMain">
      <div class="titulo">
        <h1>Desafio To Do List</h1>
      </div>
      <!--Requisito 2 | O sistema deve permitir ao usuário adicionar tarefas           concluida-->
      <div class="addTarefas">
        <form>
          <input
            type="text"
            placeholder="Adicionar tarefas"
            v-model="newTarefa"
            @keyup.enter="adicionarNovaTarefa"
          />
          <button @click="adicionarNovaTarefa">
            <i class="fas fa-plus"></i>
          </button>
        </form>
      </div>
      <!--Requisito 3 | cada tarefa adicionada deve ser exibida na lista            concluida-->
      <div class="listaTarefas">
        <ul>
          <li v-for="tarefa in tarefas" :key="tarefa.id">
            <!--Requisito 4 | O usuário deve poder marcar uma tarefa como concluída                  concluida -->
            <input type="checkbox" v-model="tarefa.completa" /><!--Requisito 5 | As tarefas concluidas devem ser diferentes visualmente                concluida -->
            <span>{{ tarefa.nome }}</span>
            <!--Requisito 6 | O usuário deve poder remover uma tarefa da lista                       concluida -->
            <button id="latinha" @click="excluirTarefa(tarefa.id)">
              <i class="far fa-trash-alt"></i>
            </button>
          </li>
        </ul>
      </div>
      <!--Requisito 7 | O sistema deverá informar quantas atividades ainda estão pendentes           concluida -->
      <div class="contadorPendentes">
        <span>Tarefas pendentes: {{ inacabadas }} </span>
      </div>
      <div class="controleBtns">
        <!--Requisito 8 | Botão para remover todas as tarefas concluídas de uma só vez       concluida-->
        <button id="botaoConfirmar" @click="excluirTarefasConcluidas">Remover tarefas concluídas</button>
        <button id="botaoConfirmar" @click="alterarTarefasConcluidas">Limpar Tarefas Concluídas</button>

      </div>
    </div>
  </div>
</template>

<script>
import "@fortawesome/fontawesome-free/css/all.css";

export default {
  name: "ToDoDesafio",
  props: {
    tarefas: Array,
  },
  data() {
    return {
      newTarefa: "", //Inicializando a variavel para uma string
      tarefasInternas: [],
      teste: false,
    };
  },
  computed: {
    inacabadas() {
      return this.tarefas.filter(this.emAndamento).length;
    },
  },
  methods: {
    emAndamento(tarefa) {
      return !this.esCompleta(tarefa);
    },
    esCompleta(tarefa) {
      return tarefa.completa;
    },

    adicionarNovaTarefa(event) {
      event.preventDefault();
      if (this.newTarefa.trim() !== "") {
        const novaTarefa = {
          id: this.tarefas.length + 1,
          nome: this.newTarefa,
          completa: false,
        };
        this.$emit("adicionar-tarefa", novaTarefa);
        this.newTarefa = "";
      }
    },

    excluirTarefa(id) {
      // Filtrar o array de tarefasInternas para remover a tarefa com o ID correspondente
      this.tarefasInternas = this.tarefasInternas.filter(
        (tarefa) => tarefa.id !== id
      );
      this.$emit("excluir-tarefa", id);
    },
    excluirTarefasConcluidas() {
      this.$emit("excluir-tarefas-concluidas");
    },
    alterarTarefasConcluidas() {
      this.tarefas.forEach((tarefa) => {
        if (tarefa.completa) {
          tarefa.completa = false;
        }
      });
    },
  },
};
</script>
