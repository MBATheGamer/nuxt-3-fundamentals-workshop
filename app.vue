<script lang="ts">
  import { defineNuxtComponent } from '#app';
import type { Todo } from './types';

  export default defineNuxtComponent({
    data: () => ({
      todos: [] as Todo[]
    }),
    methods: {
      fetchTodos() {
        fetch("https://jsonplaceholder.typicode.com/todos/")
          .then(response => response.json())
          .then(json => {
            this.todos = json as Todo[];
          });
      }
    }
  })
</script>

<template>
  <div>
    <h1>Todos</h1>
    <button @click="fetchTodos">Fetch todos</button>
    <ul>
      <li v-for="todo in todos" :key="`todo-id-${todo.id}`">
        <input type="checkbox" :checked="todo.completed"> {{ todo.title }}
      </li>
    </ul>
  </div>
</template>
