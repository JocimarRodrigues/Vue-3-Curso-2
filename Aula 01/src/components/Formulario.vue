<template>
    <div class="box">
        <div class="columns">
            <div class="column is-8" role="form" aria-label="Formulário para criação de uma nova tarefa">
                <input type="text" class="input" placeholder="Qual tarefa você deseja iniciar?">
            </div>
            <div class="column">
                <div class="is-flex is-align-items-center is-justify-content-space-between">
                    <section>
                        <strong>{{ tempoDecorrido }}</strong>
                    </section>
                    <button class="button" @click="iniciar">
                        <span class="icon">
                            <i class="fas fa-play"></i>
                        </span>
                        <span>play</span>
                    </button>
                    <button class="button" @click="finalizar">
                        <span class="icon">
                            <i class="fas fa-stop"></i>
                        </span>
                        <span>stop</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
    name: 'Formulario',
    data() {
        return {
            tempoEmSegundos: 0
        }
    },
    computed: {
        tempoDecorrido(): string {
            // return new Date(this.tempoEmSegundos * 1000).toISOString().substr(11, 8)
            // Como o método str, foi descontinuado, alterei a lógica do contador, pode causar futuros bugs nas próx aula do curso, mas aí eu resolvo. kk

            const tempoTotal = new Date(this.tempoEmSegundos * 1000);
            console.log(tempoTotal)
            const horas = ('0' + tempoTotal.getUTCHours()).slice(-2);
            const minutos = ('0' + tempoTotal.getUTCMinutes()).slice(-2);
            const segundos = ('0' + tempoTotal.getUTCSeconds()).slice(-2);
            console.log(segundos)

            return `${horas}:${minutos}:${segundos}`;


        }
    },
    methods: {
        iniciar() {
            setInterval(() => {
                this.tempoEmSegundos += 1
            }, 1000)
        },
        finalizar() {
            setInterval(() => {
                this.tempoEmSegundos -= 1
            }, 1000)
        }
    }
})
</script>

