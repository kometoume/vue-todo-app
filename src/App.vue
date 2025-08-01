.container {
  width: 400px;
  margin: 0 auto;
}

h1 {
  font-size: 20px;
  border-bottom: 1px solid;
  padding-bottom: 8px;
}
<script setup>
import { onMounted, reactive, watch } from 'vue';
import TodoList from './components/TodoList.vue';
import TodoAddForm from './components/TodoAddForm.vue';

const todos = reactive([]);

function addTodo(title) {
  todos.push({
    id: crypto.randomUUID(),
    title: title,
    isCompleted: false,
  });
}

function toggleIsCompleted(todo) {
  const index = todos.findIndex((element) => {
    return element.id === todo.id;
  });

  todos[index].isCompleted = !todos[index].isCompleted;
  // console.log(todos[index].isCompleted);
}

function deleteTodo(todo) {
  if (confirm('Sure?') === false) {
    return;
  }

  const index = todos.findIndex((element) => {
    return element.id === todo.id;
  });

  todos.splice(index, 1);
}

onMounted(() => {
  const savedTodos = localStorage.getItem('todos');

  if (savedTodos !== null) {
    const parsedTodos = JSON.parse(savedTodos);

    parsedTodos.forEach((todo) => {
      todos.push(todo);
    });
  }
});

watch(
  todos,
  (updatedTodos) => {
    localStorage.setItem('todos', JSON.stringify(updatedTodos));
  }
);
</script>

<template>
  <div class="container">
    <h1>Todos</h1>
    <TodoList
      :todos="todos"
      @checkTodoEvent="toggleIsCompleted"
      @deleteTodoEvent="deleteTodo"
    />
    <TodoAddForm @addTodoEvent="addTodo" />
  </div>
</template>

<style scoped>
.container {
  width: 400px;
  margin: 0 auto;
}

h1 {
  font-size: 20px;
  border-bottom: 1px solid;
  padding-bottom: 8px;
}
</style>