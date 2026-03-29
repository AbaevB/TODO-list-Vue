<script setup>
import { ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

// Список задач
const todos = ref([
  { id: 1, text: 'Изучить Vue', done: true },
  { id: 2, text: 'Создать TODO-приложение', done: false }
])

// Генерация уникального ID
const generateId = () => {
  return Date.now()
}

// Добавление новой задачи
const addTodo = (text) => {
  if (!text.trim()) return // не добавляем пустые

  const newTodo = {
    id: generateId(),
    text: text.trim(),
    done: false
  }

  todos.value.push(newTodo)
}

// Переключение статуса "сделано"
const toggleTodo = (id) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) {
    todo.done = !todo.done
  }
}

// Удаление задачи
const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}
</script>

<template>
  <header class="header">
    <div class="container">
      <div class="header__wrapper">
        <img alt="TODO logo" class="logo" src="./assets/logo.png" width="125" height="125" />
        <HelloWorld msg="TODO List" />
      </div>
    </div>
  </header>

  <main class="main">
    <div class="container">
      <!-- Форма добавления задачи -->
      <TodoForm @submit="addTodo" />

      <!-- Список задач -->
      <TodoList
        :todos="todos"
        @toggle="toggleTodo"
        @remove="removeTodo"
      />
    </div>
  </main>

  <footer class="footer">
    <div class="container">
      <span class="footer__copy">
        &copy; AbaevB 2026
      </span>
    </div>
  </footer>
</template>