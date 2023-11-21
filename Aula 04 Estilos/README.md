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