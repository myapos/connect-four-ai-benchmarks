# Response time 00:01:24

Response time 00:01:24
Accuracy: 8/10  
Would I use this answer? Maybe, accessibility issues span checkbox

## Notes

```vue
<template>
  <div aria-label="Todo List">
    <h1>My Todo List</h1>
    <input
      v-model="newTodo"
      @keyup.enter="addTodo"
      type="text"
      placeholder="Add a new todo"
    />
    <ul>
      <li v-for="todo in todos" :key="todo.id" aria-label="Todo Item">
        <span
          :aria-checked="todo.completed"
          role="checkbox"
          tabindex="0"
          @click="toggleComplete(todo)"
        >
          {{ todo.title }}
        </span>
        <button @click="deleteTodo(todo)" aria-label="Delete Todo">X</button>
      </li>
    </ul>
    <p>Remaining todos: {{ remainingCount }}</p>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

type Todo = {
  id: number;
  title: string;
  completed: boolean;
};

const newTodo = ref("");
let todoId = 0;
const todos = ref<Todo[]>([]);

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: todoId++,
      title: newTodo.value,
      completed: false,
    });
    newTodo.value = "";
  }
};

const toggleComplete = (todo: Todo) => {
  todo.completed = !todo.completed;
};

const deleteTodo = (todo: Todo) => {
  todos.value = todos.value.filter((t) => t.id !== todo.id);
};

const remainingCount = computed(
  () => todos.value.filter((todo) => !todo.completed).length,
);
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 5px;
}

span {
  cursor: pointer;
}

button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}
</style>
```

## Explanation

- **Type Definition**: A `Todo` type is defined with properties `id`, `title`, and `completed`.
- **Reactivity**: The component uses Vue's `ref` for reactive data, such as `newTodo` for the input field value, `todoId` to generate unique IDs for todos, and `todos` which holds the list of todos.
- **Computed Property**: `remainingCount` is a computed property that counts how many todos are not completed.
- **Methods**: `addTodo`, `toggleComplete`, and `deleteTodo` are methods used to add new todos, toggle the completion state of a todo, and delete a todo respectively.
- **Accessibility**: The component includes ARIA attributes for better accessibility, such as `aria-label`, `role="checkbox"`, and `tabindex`.
- **Styling**: Basic styling is applied using CSS scoped to the component.
