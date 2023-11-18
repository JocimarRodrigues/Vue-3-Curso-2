# Nesse projeto tu vai utilizar as dependencias Bulma e Font Awesome

- O framework Bulma, é um framework de css parecidoo com o bootstrap
- Já o font awesosome é para utilizar icones

### Bulma

- Basta tu importar essa tag, dentro do teu index.html

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css" />
```

### Font Awesome

- npm i @fortawesome/fontawesome-free para instalar o font awesome, tu precisa importar ele depois dentro do main.ts

```ts
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'

import '@fortawesome/fontawesome-free/css/all.css' // Aqui

createApp(App).mount('#app')
```

# Usando defineComponent

- Pelo que eu entendi o defineComponent era uma versão mais antigas de lidar com os export e import do Vue, ele serve basicamente para melhorar a tipagem e deixar o código mais robusto, usando TS

### Usando defineComponent

```vue
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'BarraLateral'
})
</script>
```

### Opcão do curso anterior

```vue
<script lang="ts">
import type Icategoria from '@/interfaces/ICategoria';
import type { PropType } from 'vue';
import Tag from './Tag.vue';
import IngredienteSelecionavel from './IngredienteSelecionavel.vue';

export default {
    props: {
        categoria: { type: Object as PropType<Icategoria>, required: true }
    },
    components: { Tag, IngredienteSelecionavel },
    emits: ['adicionarIngrediente', 'removerIngrediente']
}
</script>
```

## Obersevação

- Os nomes das classes das tags html, do projeto são refentes ao uso do framework Bulma, q é como se fosse um bootstrap, não vou me aprofundar em explicar elas, mas caso tu queira saber, basta dar uma lida na documentação do Bulma

- Como eu decidir usar, o esLint, na hora de criar o Componente de Formulário de acordo com a aula, o esLint está me retornando o seguinte erro no Componente de Formulário
```
Component name "Formulario" should always be multi-word.eslintvue/multi-word-component-names
```
Formulario.vue
```vue
<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
    name: 'Formulario'
})
</script>
```

- Esse erro significa que o esLint como regra quer q o nome dos Componentes tenha mais de uma palavra, para corrigir esse erro, bastaria eu renomear o nome do Componente Formulario para FormularioTarefa

- Acredito que isso seja uma convensão de nome para os componentes do Vue.


# Criando a lógica do contador

- Bom como tu já aprendeu a utilizar os methods, e diretivas no curso anterior, vou pular essa parte e vou focar no Computed

### Computed

- No computed você pode criar funções e definir uma propriedade pra elas, essas vinda do date() e o computed vai chamar a função dentro dele, toda vez q houver uma alteração na propriedade do Data

Exemplo

Formulario.vue
```vue
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
   
            const tempoTotal = new Date(this.tempoEmSegundos * 1000);
            const horas = ('0' + tempoTotal.getUTCHours()).slice(-2);
            const minutos = ('0' + tempoTotal.getUTCMinutes()).slice(-2);
            const segundos = ('0' + tempoTotal.getUTCSeconds()).slice(-2);

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
```

- Note que a função tempoDecorrida vai ocorrer toda vez q o valor da propriedade tempoEmSegundos for alterado
- Assim realizando a lógica do contador