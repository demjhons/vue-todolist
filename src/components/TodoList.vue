<template>
    <div class="container">
      <h1 class="mt-4">Todo List</h1>
      <div class="input-group mb-3">
        <input
          v-model="newTodo"
          @keyup.enter="addTodo"
          type="text"
          class="form-control"
          placeholder="Add a new task"
        />
        <button @click="addTodo" class="btn btn-primary">Add</button>
      </div>
      <ul class="list-group">
        <li
          v-for="(todo, index) in todos"
          :key="index"
          class="list-group-item d-flex align-items-center"
        >
          <div v-if="index !== editIndex" class="flex-grow-1">{{ todo }}</div>
          <input
            v-else
            v-model="editedTodo"
            @keyup.enter="updateTodo"
            @keyup.esc="cancelEdit"
            type="text"
            class="form-control"
          />
          <div class="btn-group ml-auto">
            <button
              @click="editTodo(index)"
              v-if="index !== editIndex"
              class="btn btn-primary"
            >
              Edit
            </button>
            <button @click="removeTodo(index)" class="btn btn-danger">Delete</button>
            <button
              @click="updateTodo"
              v-if="index === editIndex"
              class="btn btn-success"
            >
              Save
            </button>
            <button
              @click="cancelEdit"
              v-if="index === editIndex"
              class="btn btn-secondary"
            >
              Cancel
            </button>
          </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newTodo: '',
        todos: [],
        editIndex: -1,
        editedTodo: '',
      };
    },
    mounted() {
      // Retrieve todos from localStorage if available
      const storedTodos = localStorage.getItem('todos');
      if (storedTodos) {
        this.todos = JSON.parse(storedTodos);
      }
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim() !== '') {
          this.todos.push(this.newTodo.trim());
          this.newTodo = '';
          this.saveTodos(); // Save todos to localStorage
        }
      },
      removeTodo(index) {
        this.todos.splice(index, 1);
        this.saveTodos(); // Save todos to localStorage
      },
      editTodo(index) {
        this.editIndex = index;
        this.editedTodo = this.todos[index];
      },
      updateTodo() {
        if (this.editIndex >= 0 && this.editedTodo.trim() !== '') {
          this.todos[this.editIndex] = this.editedTodo.trim();
          this.editIndex = -1;
          this.editedTodo = '';
          this.saveTodos(); // Save todos to localStorage
        }
      },
      cancelEdit() {
        this.editIndex = -1;
        this.editedTodo = '';
      },
      saveTodos() {
        // Save todos to localStorage
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
    },
  };
  </script>