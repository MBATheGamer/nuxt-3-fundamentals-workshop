<script setup lang="ts">
  import { ref, computed } from 'vue';
  import type { Todo } from './types';

  const todos = ref<Todo[]>([]);

  const completedItems = computed(() => todos.value.filter(todo => todo.completed));

  const remainingItems = computed(() => todos.value.filter(todo => !todo.completed));

  function fetchTodos() {
    fetch("https://jsonplaceholder.typicode.com/todos/")
      .then(response => response.json())
      .then(json => {
        todos.value = json as Todo[];
      });
  }
</script>

<template>
  <div class="section">
    <h1 class="title">Todos</h1>
    <button @click="fetchTodos">Fetch todos</button>
    <p>
      {{ completedItems.length }} completed |
      {{ remainingItems.length }} remaining
    </p>
    <ul class="list">
      <li v-for="todo in todos" :key="`todo-id-${todo.id}`">
        <input type="checkbox" :checked="todo.completed"> {{ todo.title }}
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
  @import './node_modules/bulma/bulma.sass';

  $completedTodoColor: green;
  $remainingTodoColor: red;

  .list {
    color: $completedTodoColor;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
</style>
