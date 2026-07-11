# Response time 00:07:41

Response time 00:07:41
Accuracy: 8/10  
Would I use this answer? Yes  
Notes:

## Notes

```vue
<template>
  <div class="todo-app">
    <header>
      <h1>Todo List</h1>
      <p>Remaining todos: {{ remainingTodos }}</p>
    </header>
    <div class="add-todo">
      <label for="new-todo">New Todo:</label>
      <input
        id="new-todo"
        v-model="newTodoTitle"
        placeholder="Enter a new todo"
        @keyup.enter="addTodo"
      />
      <button @click="addTodo" :disabled="newTodoTitle.trim() === ''">
        Add
      </button>
    </div>
    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id" class="todo-item">
        <input
          type="checkbox"
          :checked="todo.completed"
          @click="toggleCompleted(todo.id)"
        />
        <span :class="{ completed: todo.completed }">{{ todo.title }}</span>
        <button @click="deleteTodo(todo.id)" aria-label="Delete todo">
          Delete
        </button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, computed } from "vue";

interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

const todos = reactive<Todo[]>([]);
const newTodoTitle = ref("");

function addTodo() {
  if (newTodoTitle.value.trim() === "") return;
  const newTodo: Todo = {
    id: todos.length + 1,
    title: newTodoTitle.value,
    completed: false,
  };
  todos.push(newTodo);
  newTodoTitle.value = "";
}

function deleteTodo(id: number) {
  todos.splice(
    todos.findIndex((todo) => todo.id === id),
    1,
  );
}

function toggleCompleted(id: number) {
  const todo = todos.find((todo) => todo.id === id);
  if (todo) {
    todo.completed = !todo.completed;
  }
}

const remainingTodos = computed(
  () => todos.filter((todo) => !todo.completed).length,
);
</script>

<style scoped>
.todo-app {
  max-width: 500px;
  margin: 2rem auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
}

header {
  text-align: center;
  margin-bottom: 1rem;
}

.add-todo {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.add-todo label {
  flex: 1;
}

.add-todo input {
  flex: 3;
  padding: 0.5rem;
}

.add-todo button {
  padding: 0.5rem 1rem;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 1rem;
  border-bottom: 1px solid #eee;
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-item button {
  margin-left: 1rem;
  padding: 0.3rem 0.6rem;
  cursor: pointer;
}

.completed {
  text-decoration: line-through;
  color: #888;
}
</style>
```
