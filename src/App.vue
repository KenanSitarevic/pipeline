<template>
  <div class="flex items-center justify-center">
    <div
      class="flex flex-col items-center justify-center rounded-lg shadow-lg border my-4"
    >
      <h1>My ToDo List</h1>
      <form @submit.prevent="addTodo">
        <div class="flex flex-col items-center justify-center gap-2 px-4 py-4">
          <label for="newTodo">New ToDo</label>
          <input
            v-model="newTodo"
            id="newTodo"
            name="newTodo"
            autocomplete="off"
            class="rounded-lg shadow-lg border px-2"
          />
          <button
            type="submit"
            class="rounded-lg shadow-lg border border-blue-500 py-1 px-2"
          >
            Add
          </button>
        </div>
      </form>
      <div class="py-4 flex flex-col items-center">
        <h2>ToDo List</h2>
        <ul class="flex flex-col gap-2">
          <li
            v-for="(todo, index) in todos"
            :key="index"
            class="flex flex-row justify-between gap-2 items-center"
          >
            <span :class="{ done: todo.done }" @click="toggleDone(todo)">
              {{ todo.content }}
            </span>
            <button
              class="rounded-lg shadow-lg border border-blue-500 py-1 px-2"
              @click="removeTodo(index)"
            >
              Remove
            </button>
          </li>
        </ul>
        <h4 v-if="todos.length === 0">Empty list.</h4>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

// Define the Todo type for better type safety
interface Todo {
  done: boolean;
  content: string;
}

const newTodo = ref<string>("");

const defaultData: Todo[] = [
  {
    done: false,
    content: "Write a blog post",
  },
];

// Load initial data from localStorage or use default data
const todos = ref<Todo[]>(
  (JSON.parse(localStorage.getItem("todos") || "[]") as Todo[]) || defaultData
);

// Add a new todo
function addTodo(): void {
  if (newTodo.value.trim()) {
    todos.value.push({
      done: false,
      content: newTodo.value.trim(),
    });
    newTodo.value = "";
    saveData();
  }
}

// Toggle the done status of a todo
function toggleDone(todo: Todo): void {
  todo.done = !todo.done;
  saveData();
}

// Remove a todo by index
function removeTodo(index: number): void {
  todos.value.splice(index, 1);
  saveData();
}

// Save todos to localStorage
function saveData(): void {
  localStorage.setItem("todos", JSON.stringify(todos.value));
}
</script>

<style scoped>
.done {
  text-decoration: line-through;
  color: gray;
}
</style>
