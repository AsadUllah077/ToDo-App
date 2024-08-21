<script setup>
import { ref, onMounted, watch } from 'vue';

let newTodo = ref("");
let dueDate = ref("");
let todos = ref([]);

onMounted(() => {
  const storedTodos = localStorage.getItem('todos');
  if (storedTodos) {
    todos.value = JSON.parse(storedTodos);
  }
});

watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos));
}, { deep: true });

let addTodo = () => {
  if (newTodo.value && dueDate.value) {
    todos.value.push({ item: newTodo.value, date: dueDate.value });
    newTodo.value = "";
    dueDate.value = "";
  }
};

let deleteTodo = (index) => {
  todos.value.splice(index, 1);
};
</script>

<template>
  <div class="max-w-lg mx-auto mt-10">
    <h1 class="text-center font-mono text-5xl text-blue-600 mb-8">ToDo App</h1>

    <div class="flex flex-col md:flex-row items-center justify-center space-y-4 md:space-y-0 md:space-x-4">
      <div class="w-full md:w-1/2">
        <input type="text" v-model="newTodo"
          class="w-full p-3 border rounded-lg shadow-sm focus:outline-none focus:ring focus:border-blue-300"
          placeholder="Enter a new todo item" />
      </div>
      <div class="w-full md:w-1/2">
        <input type="date" v-model="dueDate"
          class="w-full p-3 border rounded-lg shadow-sm focus:outline-none focus:ring focus:border-blue-300" />
      </div>
      <div class="w-full md:w-auto">
        <button @click="addTodo"
          class="w-full bg-blue-600 text-white py-3 px-6 rounded-lg shadow hover:bg-blue-700 focus:outline-none focus:ring focus:border-blue-300">
          Add
        </button>
      </div>
    </div>

    <div v-if="todos.length" class="mt-8">
      <div v-for="(todo, index) in todos" :key="index" class="flex flex-col md:flex-row items-center justify-between space-y-4 md:space-y-0 md:space-x-4 mb-4 shadow rounded p-4">
        <div class="w-full md:w-1/2">
          <p>{{ todo.item }}</p>
        </div>
        <div class="w-full md:w-1/2">
          <p>{{ todo.date }}</p>
        </div>
        <div class="w-full md:w-auto">
          <button @click="deleteTodo(index)"
            class="w-full bg-red-600 text-white py-3 px-6 rounded-lg shadow hover:bg-red-700 focus:outline-none focus:ring focus:border-red-300">
            Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
