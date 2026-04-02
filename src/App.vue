<script setup>
import { ref, watch, computed } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'
import TodoFilters from './components/TodoFilters.vue'
import TodoControls from './components/TodoControls.vue'
// Активный фильтр
// Читаем из localStorage, если есть; иначе — 'all'
const savedFilter = localStorage.getItem('todo-filter')
const activeFilter = ref(savedFilter || 'all')

// Список задач
const todos = ref(
  JSON.parse(localStorage.getItem('todos')) || [
    { id: 1, text: 'Изучить Vue', done: true },
    { id: 2, text: 'Создать TODO-приложение', done: false }
  ]
)

// Сохраняем todos в localStorage
watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos))
}, { deep: true })

// Сохраняем активный фильтр в localStorage
watch(activeFilter, (newFilter) => {
  localStorage.setItem('todo-filter', newFilter)
})

// Отфильтрованный список
const filteredTodos = computed(() => {
  if (activeFilter.value === 'active') {
    return todos.value.filter(todo => !todo.done)
  }
  if (activeFilter.value === 'done') {
    return todos.value.filter(todo => todo.done)
  }
  return todos.value
})

//  Вычисляемое количество активных задач
const activeCount = computed(() => {
  return todos.value.filter(todo => !todo.done).length
})

// Функция для установки фильтра - безопасна при HMR
const setFilter = (filter) => {
  activeFilter.value = filter
}

// Генерация ID
const generateId = () => Date.now()

// Добавление задачи
const addTodo = (text) => {
  if (!text.trim()) return
  todos.value.push({
    id: generateId(),
    text: text.trim(),
    done: false
  })
}

// Переключение статуса
const toggleTodo = (id) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) todo.done = !todo.done
}

// Удаление
const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

const clearDone = () => {
  todos.value = todos.value.filter(todo => !todo.done)
}

const hasCompleted = computed(() => {
  return todos.value.some(todo => todo.done)
})


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
      <TodoForm @submit="addTodo" />

      <TodoFilters :active-filter="activeFilter.value" @filter-change="setFilter" />

      <TodoControls :active-count="activeCount" :has-completed="hasCompleted" @clear-done="clearDone" />

      <TodoList :todos="filteredTodos" @toggle="toggleTodo" @remove="removeTodo" />
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