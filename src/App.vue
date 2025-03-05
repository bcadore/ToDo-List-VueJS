<script setup>
import { reactive } from "vue";
import Cabecalhos from "./components/Cabecalho.vue";
import Formulario from "./components/Formulario.vue";
import ListaTarefas from "./components/ListaTarefas.vue";

const estado = reactive({
  filtro: "todas",

  tarefaTemp: "",

  tarefas: [
    {
      titulo: "Ir na academina",
      finalizada: true,
    },
    {
      titulo: "Trabalhar",
      finalizada: true,
    },
    {
      titulo: "Estudar ES6",
      finalizada: false,
    },
    {
      titulo: "Preparar a janta",
      finalizada: false,
    },
  ],
});

const getTarefasPendentes = () => {
  return estado.tarefas.filter((tarefa) => !tarefa.finalizada);
};

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter((tarefa) => tarefa.finalizada);
};

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case "pendentes":
      return getTarefasPendentes();
    case "finalizadas":
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
      break;
  }
};

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  };
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = "";
};
</script>

<template>
  <div class="container">
    <Cabecalhos :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario
      :tarefa-temp="estado.tarefaTemp"
      :edita-tarefa-temp="(evento) => (estado.tarefaTemp = evento.target.value)"
      :cadastra-tarefa="cadastraTarefa"
      :trocar-filtro="evento => estado.filtro = evento.target.value"
    />
    <ListaTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>
