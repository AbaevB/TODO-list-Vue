<script setup>
import { ref, watch, computed } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

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

// Активный фильтр
const activeFilter = ref('all')
console.log('is ref:', activeFilter.value !== undefined)

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

      <!-- Кнопки фильтров -->
      <div class="filters">
        <button
          :class="{ active: activeFilter.value === 'all' }"
          @click="setFilter('all')"
        >
          Все
        </button>
        <button
          :class="{ active: activeFilter.value === 'active' }"
          @click="setFilter('active')"
        >
          Активные
        </button>
        <button
          :class="{ active: activeFilter.value === 'done' }"
          @click="setFilter('done')"
        >
          Выполненные
        </button>
      </div>

      <TodoList
        :todos="filteredTodos"
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