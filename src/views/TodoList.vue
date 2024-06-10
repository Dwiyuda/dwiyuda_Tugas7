<!-- src/views/TodoList.vue -->
<template>
    <div class="todo-container">
      <h1>Todo List</h1>
      <form @submit.prevent="addNewTodo">
        <input v-model="newTodo" placeholder="Add a new task" />
        <button type="submit">Add</button>
      </form>
      <ul>
        <li v-for="(todo, index) in todos" :key="index">
          <span :class="{ completed: todo.completed }" @click="toggleTodoStatus(index)">
            {{ todo.text }}
          </span>
          <button @click="deleteTodo(index)">Delete</button>
        </li>
      </ul>
      <p>Incomplete tasks: {{ incompleteTodosCount }}</p>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import { useTodoStore } from '../stores/todoStore';
  
  export default {
    setup() {
      const newTodo = ref('');
      const todoStore = useTodoStore();
  
      const addNewTodo = () => {
        if (newTodo.value.trim()) {
          todoStore.addTodo(newTodo.value);
          newTodo.value = '';
        }
      };
  
      const deleteTodo = (index) => {
        todoStore.removeTodo(index);
      };
  
      const toggleTodoStatus = (index) => {
        todoStore.toggleTodo(index);
      };
  
      return {
        newTodo,
        todos: todoStore.todos,
        addNewTodo,
        deleteTodo,
        toggleTodoStatus,
        incompleteTodosCount: todoStore.incompleteTodosCount
      };
    }
  };
  </script>
  
  <style scoped>
  .todo-container {
    max-width: 400px;
    margin: 0 auto;
    padding: 1em;
    border-radius: 8px;
    background-color: #f9f9f9;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    text-align: center;
    color: #333;
  }
  
  form {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1em;
  }
  
  input {
    flex: 1;
    padding: 0.5em;
    margin-right: 0.5em;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  
  button {
    padding: 0.5em 1em;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #fff;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  li {
    display: flex;
    justify-content: space-between;
    padding: 0.5em 0;
    border-bottom: 1px solid #ddd;
  }
  
  .completed {
    text-decoration: line-through;
    color: #999;
  }
  </style>
  