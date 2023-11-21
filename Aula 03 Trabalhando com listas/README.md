# Criando lógica de adicionar uma tarefa a uma lista

## 1 Como vai ser uma lista tu vai criar um componente para renderizar essa lista usando o v-for

Tarefeas.vue
```vue
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
```

- Nesse componente tu vai definir que ele vai receber uma props q vai ser a tarefa, q tu vai criar no componente pai
- Note que tu vai usar o Proptype para tipar o objeto, usando a interface

## 2 Tu vai chamar o componente, passar a props, criar a lógica da lista e fazer o v-for

- No App.vue q  é o componente pai tu vai chamar o Componente Tarefa, passando pra ele a Props q é a lista de tarefas
- No Componente do App.vue, tu também vai criar a lista de taarefas
- Esse também é o componente responsável q tu vai criar a lógica para adicionar o item na lista

App.vue
```vue


<template>
  <main class="columns is-gapless is-multiline">
    <div class="column is-one-quarter">
      <BarraLateral />
    </div>
    <div class="column is-three-quarter">
      <Formulario @aoSalvarTarefa="salvarTarefa"/> <!--Aqui-->
      <div class="lista">
        <Tarefa v-for="(tarefa, index) in tarefas" :key="index" :tarefa="tarefa"/> <!--Aqui-->
      </div>
    </div>

  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import BarraLateral from './components/BarraLateral.vue';
import Formulario from './components/Formulario.vue';
import Tarefa from './components/Tarefa.vue';
import type ITarefa from './interfaces/ITarefa'

export default defineComponent({
  name: 'App',
  components: {
    BarraLateral,
    Formulario,
    Tarefa
  },
  data() {
    return {
      tarefas: [] as ITarefa[]
    }
  },
  methods: {
    salvarTarefa(tarefa: ITarefa) { // Aqui
      this.tarefas.push(tarefa)
    }
  }
})
</script>

<style scoped>
.lista {
  padding: 1.25rem;
}
</style>

```

### Obersavação

- Note que no v-for tu pode pegar o index da lista, exatamente como um for, do js; tu vai usar esse index como key pra lista

### Importante

- Note também que tu está usando o componente Formulário para salvar o nome/descricao e enviar para o array da lista
- Assim concluindo a lógica.

- Caso tenha dúvidas o link da aula é esse => https://cursos.alura.com.br/course/vue3-comecando-framework/task/97501

# Definindo um valor padrao ao chamar  uma props

Tarefa.vue
```vue
<template>
    <div class="box has-text-weight-bold">
        <div class="columns">
            <div class="column is-7">
                {{ tarefa.descricao || 'Tarefa sem descricao' }}
            </div>
            <div class="column">
                <Cronometro :tempoEmSegundos="tarefa.duracaoEmSegundos" />
            </div>
        </div>
    </div>
</template>
```

- Tu pode usar o || para definir um valor padrao para a props, caso ela não venha.

# Sobre o Slot

- Olha o que aconteceu, tu criou um novo componente para refatorar o código e colocou todo o conteúdo do template de Tarefa, dentro dele, mas aconteceu um bug, que ao adicionar as tarefas, o conteúdo ficava em branco.
- Para resolver esse problema tu vai usar o slot, q é como se fosse um "children", isso é: Vai renderizar todo o conteúdo que vai ficar dentro do componente

Box.vue
```vue
<template>
    <div class="box has-text-weight-bold">
        <slot></slot>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
    name: 'Box'
})
</script>

<style scoped>
.box {
    background: #FAF0CA;
}
</style>
```

Tarefa.vue
```vue
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
```
