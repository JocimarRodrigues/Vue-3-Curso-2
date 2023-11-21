# Preparando o modo escuro

## Usando variaveis css
App.vue
```vue

<template>
  <main class="columns is-gapless is-multiline modo-escuro"> <!--Aqui-->
    <div class="column is-one-quarter">
      <BarraLateral />
    </div>
    <div class="column is-three-quarter">
      <Formulario @aoSalvarTarefa="salvarTarefa"/>
      <div class="lista">
        <Tarefa v-for="(tarefa, index) in tarefas" :key="index" :tarefa="tarefa"/>
        <Box v-if="listaEstaVazia">
          Ainda não foram adicionada tarefas.
        </Box>
      </div>
    </div>

  </main>
</template>


<style scoped>
.lista {
  padding: 1.25rem;
}

main {
  --bg-primario: #fff; /* Aqui */
  --texto-primario: #000;

}

main.modo-escuro {
  --bg-primario: #2b2d42;
  --texto-primario: #ddd;
}

.conteudo {
  background-color: var(--bg-primario); /* Usando a variavel */
}
</style>

```

- Basta tu definir a variavel com -- e o nome da variavel
- Depois quando tu quiser usar, tu vai usar var(nome da variavel)

## Passando as variaveis css entre os componentes

- Note que tu criou as variaveis dentro da tag main, portanto todo componente q estiver dentro dessa tag, vai poder receber essas variaveis
- Para usar ela em outro componente, basta abrir o var(nome da variavel) e definir o nome da variavel q tu criou no main

Exemplo

Formulario.vue

```vue

<template>
    <div class="box formulario"> <!--Aqui-->
        <div class="columns">
            <div class="column is-8" role="form" aria-label="Formulário para criação de uma nova tarefa">
                <input type="text" class="input" placeholder="Qual tarefa você deseja iniciar?" v-model="descricao">
            </div>
            <div class="column">
                <Temporizador @aoTemporizadorFinalizado="finalizarTarefa"/>
            </div>
        </div>
    </div>
</template>

<style>
.formulario {
    color: var(--texto-primario); /* Aqui */
    background-color: var(--bg-primario);
}
</style>
```

- Note que tu está usando as variaveis q tu criou lá dentro do main no App.vue

# Criando botao para mudar o tema

### 1 Tu vai criar a lógica dentro do componente BarraLateral

BarraLateral.vue
```vue
<template>
    <header>
        <h1><img src="../assets/logo.png" alt="Logo alura  tracker"></h1>
        <button class="button" @click="alterarTema">{{ textoBotao }}</button> <!--Aqui-->
    </header>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'BarraLateral',
    emits: ['aoTemaAlterado'], // Aqui
    data() { // Aqui
        return {
            modoEscuroAtivo: false
        }
    },
    computed: { // Aqui
        textoBotao () {
            if(this.modoEscuroAtivo) {
                return 'Desativar modo Escuro'
            }
            return 'Ativar modo escuro'
        }
    },
    methods: {
        alterarTema () { // Aqui
            this.modoEscuroAtivo = !this.modoEscuroAtivo
            this.$emit('aoTemaAlterado', this.modoEscuroAtivo)
        }
    }
})
</script>

<style scoped>
header {
    pad: 1rem;
    background: #0d3b66;
    widows: 100%;
    height: 100vh;
    text-align: center;
}

@media only screen and (max-width: 768px) {
    header {
        padding: 2.5rem;
        height: auto;
    }
}
</style>
```

### O que está acontecendo

- Tu criou um state boolean para receber o estado do tema
- Tu criou um computed para ficar analisando se esse state vai mudar
- Dentro de methods tu criou a funcao q muda esse state
- Definiu com as chaves para receber a funcao do computed q vai receber os valores de string Desativar modo Escuro, Ativar modo Escuro; Esses valores vão depender da funcao alterarTema, por isso o computed

# Dentro do App.vue tu vai chamar o evento personalizado aoTemaAlterado

App.vue
```vue


<template>
  <main class="columns is-gapless is-multiline" :class="{ 'modo-escuro': modoEscuroAtivo }"> <!--Aqui-->
    <div class="column is-one-quarter">
      <BarraLateral @aoTemaAlterado="trocarTema" /> <!--Aqui-->
    </div>
    <div class="column is-three-quarter conteudo">
      <Formulario @aoSalvarTarefa="salvarTarefa" />
      <div class="lista">
        <Tarefa v-for="(tarefa, index) in tarefas" :key="index" :tarefa="tarefa" />
        <Box v-if="listaEstaVazia">
          Ainda não foram adicionada tarefas.
        </Box>
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
import Box from './components/Box.vue';

export default defineComponent({
  name: 'App',
  components: {
    BarraLateral,
    Formulario,
    Tarefa,
    Box
  },
  data() {
    return {
      tarefas: [] as ITarefa[],
      modoEscuroAtivo: false // Aqui
    }
  },
  computed: {
    listaEstaVazia(): boolean {
      return this.tarefas.length === 0
    }
  },
  methods: {
    salvarTarefa(tarefa: ITarefa) {
      this.tarefas.push(tarefa)
    },
    trocarTema(modoEscuroAtivo: boolean) { // Aqui
      this.modoEscuroAtivo = modoEscuroAtivo
    }
  }
})
</script>
```

### O que está acontecendo aqui

- 1 Você está chamando aquele evento personalizado q tu criou dentro do Componente BarraLatera.vue o aoTemaAlterado
- 2 Tu está paassando pra ele a funcao trocarTema, q vai pegar um state q tu criou dentro desse componente q é o modoEscuroAtivo; Aí dentro da funcao aoTemaAlterado, q vai receber o state, vai realizar a lógica q sempre vai receber o contrário do modoEscuroAtivo, usando o emit
- 3 Tu vai usar o :class para usar javascript dentro do código da classe e como tu realizou a lógica de boolean a classe modo-escuro só vai existir se modoEscuroAtivo for true

- Se tiver dúvidas o link da aula é esse => https://cursos.alura.com.br/course/vue3-comecando-framework/task/97504


# Apliccando estilo com objetos

- Isso funciona semelhante ao css inline, tu vai usar o bind, criar uma classe de estilo e dentro dessa classe tu vai definir o objeto de estilos
- Depois, basta entrar no método data(), criar o objeto de estilo e definir os estilos como se fosse o css normal

Exemplo


Utilizando Objetos
Box.vue
```vue
<template>
    <div class="box has-text-weight-bold" :style="estilos">
        <slot></slot>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
    name: 'Box',
    data() {
        return {
            estilos: {
                background: '#FAF0CA'
            }
        }
    }
})
</script>


```

Forma padrão
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

## Observação

- Caso a propriedade tenha o -, por exemplo background-color tu pode resolver de duas formas

### Forma 1

- Utilizando camelCase = escrevendo assim => backgroundColor: #FAF0CA

### Forma 2

- Utilizando '' = 'background-color': '#FAF0CA'