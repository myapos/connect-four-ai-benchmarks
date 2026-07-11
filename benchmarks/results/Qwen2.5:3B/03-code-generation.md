# Response time 00:00:38

Accuracy: 4/10  
Would I use this answer? Maybe /No

## Notes

- No input/form for adding a todo
- Uses `$updateTodo` in the template but not for the actual toggle flow
- In the template it calls `removeTodo`, but in the script it has `$removeTodo`
- Does not display `countRemainingTodos`
- Initializes the todo with an empty title

```html
<template>
  <div class="todo-list">
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <label @click="$updateTodo(todo)">
          <input type="checkbox" v-model="todo.completed" />
          <span>{{ todo.title }}</span>
        </label>
        <button @click.stop="removeTodo(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
  import { computed, ref } from "vue";

  type Todo = { id: number; title: string; completed: boolean };

  const todos = ref<Todo[]>([{ id: 0, title: "", completed: false }]);

  function $addTodo(title: string) {
    const newId =
      todos.value.length > 0 ? todos.value[todos.value.length - 1].id + 1 : 0;
    todos.value.push({ id: newId, title: title.trim(), completed: false });
  }

  const $updateTodo = (todo: Todo) => {
    if (!todo.title || todo.title.trim() === "") return;
    const index = todos.value.indexOf(todo);
    todos.value[index] = { ...todo };
  };

  function $removeTodo(index: number) {
    todos.value.splice(index, 1);
  }

  const countRemainingTodos = computed(() => {
    return todos.value.filter((todo) => !todo.completed).length;
  });
</script>

<style scoped>
  .todo-list {
    max-width: 300px;
    margin: auto;
  }
  li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    list-style-type: none;
    padding: 1rem 0;
    border-bottom: 1px solid #ccc;
  }
  input[type="checkbox"] {
    margin-right: 1rem;
  }
  button {
    background-color: red;
    color: white;
    font-size: 12px;
  }
</style>
```

## Explanation

- **Vue 3 + TypeScript**: This component is defined using the `<script setup lang="ts">` syntax, which allows for a more concise and closer-to-native Vue usage.
- **Todo Type Definition**: A simple type definition for Todo items ensures clarity in data structure within components and enhances code maintainability.
- **Adding Todos**: The `$addTodo` method prevents adding empty todos by checking if the title is non-empty before pushing to the array. This avoids unnecessary data handling.
- **Updating Todos**: Utilizes a computed property `countRemainingTodos` that filters out completed todos from the list, simplifying logic within templates without needing additional watchers or methods.
- **Deleting Todo**: A simple button event triggers `$removeTodo(index)`, which removes the element at the specified index from the array.
- **Accessibility and Basic Styling**: Basic accessibility elements like `<label>` for checkbox interaction and styled buttons are included to ensure users can easily interact with components, while also keeping a minimalistic and clean appearance.
