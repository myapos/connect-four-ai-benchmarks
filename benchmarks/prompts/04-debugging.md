You are a senior frontend engineer.

The following Vue 3 + TypeScript component has a bug: when clicking the checkbox, the todo sometimes toggles twice or ends up in the wrong completed state.

Find the root cause, explain why it happens, and provide the corrected code.

```vue
<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id" @click="toggleTodo(todo.id)">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="toggleTodo(todo.id)"
      />
      <span :class="{ completed: todo.completed }">
        {{ todo.title }}
      </span>
      <button @click="deleteTodo(todo.id)">Delete</button>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { ref } from "vue";

type Todo = {
  id: number;
  title: string;
  completed: boolean;
};

const todos = ref<Todo[]>([
  { id: 1, title: "Learn Vue", completed: false },
  { id: 2, title: "Write tests", completed: false },
]);

function toggleTodo(id: number) {
  const todo = todos.value.find((todo) => todo.id === id);
  if (todo) {
    todo.completed = !todo.completed;
  }
}

function deleteTodo(id: number) {
  todos.value = todos.value.filter((todo) => todo.id !== id);
}
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>
```
