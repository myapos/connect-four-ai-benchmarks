# Accuracy: 4/10

Response time 00:01:21
Accuracy: 4/10  
Would I use this answer? No, has bugs  
Notes:

## Notes

```vue
<template>
  <div class="todo-list">
    <h1>Todo List</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo.title" placeholder="New Todo" />
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <input
          type="checkbox"
          v-model="todo.completed"
          @change="updateTodo(todo)"
        />
        <span>{{ todo.title }}</span>
        <button @click="deleteTodo(todo)">Delete</button>
      </li>
    </ul>
    <p>Remaining: {{ remainingCount }}</p>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

const todos = ref([
  { id: 1, title: "Todo 1", completed: false },
  { id: 2, title: "Todo 2", completed: true },
]);
let newTodoTitle = "";

function addTodo() {
  if (newTodoTitle.trim() === "") return;
  todos.value.push({
    id: todos.value.length + 1,
    title: newTodoTitle,
    completed: false,
  });
  newTodoTitle = "";
}

function updateTodo(todo: Todo) {
  todo.completed = !todo.completed;
}

function deleteTodo(todo: Todo) {
  const index = todos.value.findIndex((t) => t.id === todo.id);
  if (index !== -1) todos.value.splice(index, 1);
  remainingCount = todos.value.filter((t) => !t.completed).length;
}

const remainingCount = computed(
  () =>
    todos.value.length - todos.value.filter((todo) => !todo.completed).length,
);
</script>

<style scoped>
.todo-list {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.todo-list h1 {
  margin-bottom: 20px;
}

.todo-list form {
  display: flex;
  align-items: center;
  padding-bottom: 10px;
  border-bottom: 1px solid #ccc;
}

.todo-list input[type="text"] {
  width: 80%;
  padding: 10px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  background-color: #f2f2f2;
}

.todo-list button[type="submit"] {
  width: 20%;
  padding: 10px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  background-color: #4caf50;
  color: #fff;
  cursor: pointer;
}

.todo-list li {
  margin-bottom: 10px;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  display: flex;
  align-items: center;
}

.todo-list input[type="checkbox"] {
  width: 20px;
  height: 20px;
}

.todo-list span {
  margin-left: 10px;
}

.todo-list button {
  margin-left: 10px;
}
</style>
```

## Design Decisions

The main design decisions for this Todo List component are:

- **Use of Basic Accessible Markup**: The use of `<form>` and `input` elements with placeholder text to provide a clear user interface.
- **Type Definitions**: Using TypeScript to define the `Todo` type, which includes properties like `id`, `title`, and `completed`.
- **Ref Management**: Using the `ref` function from Vue's Composition API to manage state in a reactive way.
- **Computed Properties**: Creating a computed property for the `remainingCount` to display the number of remaining todos.
