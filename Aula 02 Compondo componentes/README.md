# Documentação Date API

- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Date

# Criando lógica para usar o input do formulário e passando o state do filho pro pai

## 1 Para tu passar o state de um componente filho pra pai, tu vai usar o $emit

Temporizador.vue
```vue
<template>
    <div class="is-flex is-align-items-center is-justify-content-space-between">
        <Cronometro :tempo-em-segundos="tempoEmSegundos" />
        <button class="button" @click="iniciar" :disabled="cronometroRodando">
            <span class="icon">
                <i class="fas fa-play"></i>
            </span>
            <span>play</span>
        </button>
        <button class="button" @click="finalizar" :disabled="!cronometroRodando">
            <span class="icon">
                <i class="fas fa-stop"></i>
            </span>
            <span>stop</span>
        </button>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Cronometro from './Cronometro.vue';

export default defineComponent({
    name: 'Temporizador',
    emits: ['aoTemporizadorFinalizado'], // Aqui
    components: {
        Cronometro
    },
    data() {
        return {
            tempoEmSegundos: 0,
            cronometro: 0,
            cronometroRodando: false
        }
    },
    methods: {
        iniciar() {
            this.cronometroRodando = true
            this.cronometro = setInterval(() => {
                this.tempoEmSegundos += 1
            }, 1000)
        },
        finalizar() {
            this.cronometroRodando = false
            clearInterval(this.cronometro)
            this.$emit('aoTemporizadorFinalizado', this.tempoEmSegundos) // Aqui
            this.tempoEmSegundos = 0

        }
    }
})
</script>
```

### O que está acontecendo aqui?

- Tu define o emit, dando um nome para o teu evento personalizado, lembrar que precisa ser feito em [nome do evento]
- Depois dentro de uma função/lógica  tu vai chamar o meoto $emit e o 1 parametro é o nome do evento que tu criou lá em cima
- O segundo parametro é o dado que tu quer q esse evento passe

## 2 Dentro do componente pai tu vai criar a lógica para usar esse evento

Formulario.vue
```vue
<template>
    <div class="box">
        <div class="columns">
            <div class="column is-8" role="form" aria-label="Formulário para criação de uma nova tarefa">
                <input type="text" class="input" placeholder="Qual tarefa você deseja iniciar?" v-model="descricao">
            </div>
            <div class="column">
                <Temporizador @aoTemporizadorFinalizado="finalizarTarefa"/> <!--Aqui-->
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Temporizador from './Temporizador.vue'

export  default defineComponent({
    name: "Formulário",
    components: {
        Temporizador
    },
    data() {
        return {
            descricao: ''
        }
    },
    methods: {
        finalizarTarefa(tempoDecorrido: number) : void { // Aqui
            console.log('Tempo da tarefa', tempoDecorrido)
            console.log('descricao da tarefa', this.descricao)
            this.descricao = ''
        }
    }
})
</script>
```

### O que está acontecendo aqui

- 1 Tu está chamando aquele componente filho e com o uso do @, tu está chamando aquele evento personalizado que tu criou lá, passando para ele uma função que tu vai criar nesse componente
- 2 A funcao que tu criou nesse componente recebe como parametro o dado do teu evento personalizado
- 3 Como é uma função q não retorna nada o método dela pode ser void
- 4 Note  também q tu criou um state nesse componente chamada descricao q é para pegar os dados do input
- 5 Tu usou a diretiva v-model para pegar os dados e atribiu o dado direto no state descricao q tu criou, dentro de date.