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
