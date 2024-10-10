<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

  // Estado reativo para armazenar tarefas e a tarefa temporária
  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar Es6',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true,
      },
    ],
  });

  // Funções para filtrar tarefas
  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
  };

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
  };

  const getTarefasFiltradas = () => {
    switch (estado.filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  };

  // Função para cadastrar a nova tarefa
  const cadastraTarefa = () => {
    if (!estado.tarefaTemp.trim()) {
      return; // Não adicionar tarefa se o campo estiver vazio
    }
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    };
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = ''; // Limpa o campo temporário
  };
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <!-- Escutar o evento update:tarefaTemp do componente filho -->
    <Formulario 
      :tarefa-temp="estado.tarefaTemp" 
      @update:tarefaTemp="estado.tarefaTemp = $event"
      :trocar-filtro="evento => estado.filtro = evento.target.value"  
      :cadastra-tarefa="cadastraTarefa"
    />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>