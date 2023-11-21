<template>
    <div class="box has-text-weight-bold">
        <div class="columns">
            <div class="column is-7">
                {{ tarefa.descricao }}
            </div>
            <div class="column">
                <Cronometro :tempoEmSegundos="tarefa.duracaoEmSegundos" />
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, Proptype } from 'vue';
import Cronometro from './Cronometro.vue';
import type ITarefa from '../interfaces/ITarefa'

export default defineComponent({
    name: 'Tarefa',
    components: {
        Cronometro,
    },
    props: {
        tarefa: {
            type: Object as Proptype<ITarefa>,
            required: true
        }
    }
})
</script>

<style scoped>
.box {
    background: #FAF0CA;
}
</style>