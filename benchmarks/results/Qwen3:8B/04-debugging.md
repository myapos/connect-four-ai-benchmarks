Response time 00:05:39
Accuracy: 10/10  
Would I use this answer? Yes  
Notes:

The root cause of the bug is that **both the `@click` on the `<li>` and the `@change` on the checkbox trigger the same `toggleTodo` function**, leading to **two toggles** when the checkbox is clicked.

This causes the `completed` state to flip twice, resulting in the wrong state or unexpected behavior.

### **Explanation of the Issue**

- When the checkbox is clicked:

1. The `@change` event on the checkbox triggers `toggleTodo`.
2. The `@click` event on the `<li>` also triggers `toggleTodo`.

- This results in **two invocations of `toggleTodo`** for a single checkbox click, flipping the `completed` state twice (e.g., `false → true → false`).

### **Corrected Code**

To fix this, **prevent the `<li>`'s click event from propagating** when the checkbox is clicked.

This ensures only one toggle occurs.

```vue
<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id" @click="toggleTodo(todo.id)">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="
          (e) => {
            toggleTodo(todo.id);
            e.stopPropagation();
          }
        "
      />
      <span :class="{ completed: todo.completed }">{{ todo.title }}</span>
      <button @click="deleteTodo(todo.id)">Delete</button>
    </li>
  </ul>
</template>
```

### **Key Fix**

- Added `e.stopPropagation()` to the checkbox's `@change` handler to **stop the click event from bubbling up** to the `<li>`, preventing the double toggle.

### **Why This Works**

- The checkbox's `@change` event is the **correct place** to handle the toggle logic.
- The `<li>`'s `@click` is redundant and causes unintended behavior when the checkbox is clicked.
- By stopping propagation, we ensure the `<li>`'s click handler is **not triggered** when the checkbox is clicked, resolving the bug.
