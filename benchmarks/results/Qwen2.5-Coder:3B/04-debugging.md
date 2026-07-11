Response time 00:00:48
Accuracy: 7/10  
Would I use this answer? Yes
Notes: preventDefault does not solve the problem. The issue is propagation. Then it changes the whole toggle logic. So it understood the symptom, but not the root cause fully.

The root cause of the bug is that the `change` event handler is being called twice when the checkbox is toggled.

This happens because the checkbox's state is being updated immediately as it's clicked, causing the `toggleTodo` function to be called again.

To fix this issue, we can prevent the default behavior of the checkbox by calling `event.preventDefault()` in the `change` event handler.

Additionally, we should ensure that the `toggleTodo` function only toggles the completed state if the checkbox is actually checked or unchecked, rather than just toggling it on every click.

Here's the corrected code:

```vue
<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id" @click="toggleTodo(todo.id)">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change.prevent="toggleTodo(todo.id, event)"
      />
      <span :class="{ completed: todo.completed }">{{ todo.title }}</span>
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

function toggleTodo(id: number, event?: Event) {
  const todo = todos.value.find((todo) => todo.id === id);
  if (todo && event?.target instanceof HTMLInputElement) {
    todo.completed = !event.target.checked;
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

### Explanation:

1. **Prevent Default Behavior**: The `@change.prevent` directive is used to prevent the default behavior of the checkbox, which triggers a click event.

2. **Check for Checkbox State**: Before toggling the completed state, we check if the target element (`event.target`) is an `HTMLInputElement`. This ensures that we only toggle the state when the user actually interacts with the checkbox.

3. **Toggle Logic**: The logic inside the `toggleTodo` function now checks both the event's `target.checked` property and the checkbox's `completed` property to ensure it toggles correctly.

With these changes, the checkbox should work as expected without toggling twice or ending up in the wrong completed state.
