<script setup>
    const props = defineProps(['cadastraTarefa', 'tarefaTemp', 'trocarFiltro']);
    const emit = defineEmits(['update:tarefaTemp']);

    // Função para emitir o valor da tarefa temporária
    const handleInputChange = (evento) => {
        emit('update:tarefaTemp', evento.target.value);
    }
</script>

<template>
    <form @submit.prevent="props.cadastraTarefa">
        <div class="row">
            <div class="col">
                <!-- Usar @input para atualizar o valor da tarefa -->
                <input 
                    :value="props.tarefaTemp" 
                    @input="handleInputChange" 
                    required 
                    type="text" 
                    placeholder="Digite aqui a descrição da tarefa" 
                    class="form-control"
                />
            </div>
            <div class="col-md-2">
                <button type="submit" class="btn btn-primary">Cadastrar</button>
            </div>
            <div class="col-md-2">
                <select @change="props.trocarFiltro" class="form-control">
                    <option value="todas">Todas tarefas</option>
                    <option value="pendentes">Pendentes</option>
                    <option value="finalizadas">Finalizadas</option>
                </select>
            </div>
        </div>
    </form>
</template>