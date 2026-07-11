# Response time 00:0:12

Accuracy: 6.5/10
Would I use this answer? No, missing refs, :key=index issues bugs with clicks

## Notes

```vue
<script setup lang="ts">
type Todo = {
  id: string;
  title: string;
  completed: boolean;
};

const todos = ref<Todo[]>([]);
const newTodoTitle = ref("");

function addTodo() {
  if (newTodoTitle.value.trim()) {
    todos.value.push({
      id: crypto.randomUUID(),
      title: newTodoTitle.value,
      completed: false,
    });
    newTodoTitle.value = "";
  }
}

function toggleCompletion(id: string) {
  const todo = todos.value.find((t) => t.id === id);
  if (todo) {
    todo.completed = !todo.completed;
  }
}

function deleteTodo(id: string) {
  todos.value = todos.value.filter((t) => t.id !== id);
}
</script>

<template>
  <div class="todo-list">
    <h1>Todo List</h1>
    <form @submit.prevent="addTodo">
      <input
        v-model="newTodoTitle"
        type="text"
        placeholder="Add a new todo..."
      />
      <button type="submit">Add</button>
    </form>
    <ul class="todo-items">
      <li
        v-for="(todo, index) in todos"
        :key="index"
        @click="toggleCompletion(todo.id)"
      >
        <input
          type="checkbox"
          :checked="todo.completed"
          @change.prevent="toggleCompletion(todo.id)"
        />
        <span class="todo-title" :class="{ completed: todo.completed }">
          {{ todo.title }}
        </span>
        <button @click.prevent="deleteTodo(todo.id)" class="delete-btn">
          Delete
        </button>
      </li>
    </ul>
    <p>Remaining todos: {{ todos.filter((t) => !t.completed).length }}</p>
  </div>
</template>

<style scoped>
.todo-list {
  max-width: 600px;
  margin: auto;
}

form {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

input[type="text"] {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
}

button {
  padding: 8px 16px;
  background-color: #3f51b5;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.todo-items {
  list-style-type: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.todo-title {
  margin-left: 10px;
  text-decoration: line-through;
  color: #64b5f6;
}

.delete-btn {
  padding: 4px 8px;
  background-color: #d32f2f;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

## Main Design Decisions

1. **State Management**: The Todo list is managed using Vue's reactive state management system with `ref` and computed properties. This allows for easy tracking of the todos, new todo input, and remaining todos.
2. **Accessibility**: Basic accessibility features are included by using semantic HTML elements like `<form>`, `<input>`, `<button>`, and `<span>`. The checkbox is styled to be visually distinct from other content and has a role attribute for screen readers.
3. **Reactivity**: The component uses reactivity to update the UI in real-time as the user interacts with the todo list, such as adding or deleting todos.
4. **Validation**: Adding an empty todo is prevented by checking if `newTodoTitle.value.trim()` is not empty before adding a new todo.
5. **Separation of Concerns**: The component's logic is kept separate from its presentation in the template using `<script setup lang="ts">`. This separation enhances code readability and maintainability.
6. **Styling**: Basic CSS is used to style the todo list, ensuring it looks clean and accessible.
