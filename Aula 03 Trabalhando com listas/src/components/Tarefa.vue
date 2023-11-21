<template>
    <Box>
        <div class="columns">
            <div class="column is-7">
                {{ tarefa.descricao }}
            </div>
            <div class="column">
                <Cronometro :tempoEmSegundos="tarefa.duracaoEmSegundos" />
            </div>
        </div>
    </Box>
</template>

<script lang="ts">
import { defineComponent, Proptype } from 'vue';
import Cronometro from './Cronometro.vue';
import type ITarefa from '../interfaces/ITarefa'
import Box from './Box.vue'

export default defineComponent({
    name: 'Tarefa',
    components: {
        Cronometro,
        Box
    },
    props: {
        tarefa: {
            type: Object as Proptype<ITarefa>,
            required: true
        }
    }
})
</script>

