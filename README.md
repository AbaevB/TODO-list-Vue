# Todo-list

> Учебный проект - планировщик задач на фреймворке Vue 3 с использованием `<script setup>` и SASS.

## 🚀 Первый этап: Верстка и базовая структура

Цель первого этапа - создать **работающую структуру приложения**:

- Компоненты с правильной разметкой
- Стили по методологии БЭМ
- Подключение в корневом компоненте
- Подготовка к добавлению логики

---

### 1. Создание компонентов

Создаём папку `src/components/` и следующие файлы:

src/
└── components/
├── TodoForm.vue
├── TodoList.vue
└── TodoItem.vue

Каждый компонент пока содержит **только разметку и стили** - логика будет добавлена позже.

---

### 2. Верстка компонентов

#### `TodoForm.vue` - форма добавления задачи

```html
<template>
  <form class="todo-form">
    <input
      type="text"
      placeholder="Что нужно сделать?"
      class="todo-form__field"
      aria-label="Новая задача"
    >
    <button type="submit" class="todo-form__btn">Добавить</button>
  </form>
</template>

```

**Особенности:**

- Форма с полем ввода и кнопкой
- Используются классы по БЭМ: todo-form__field, todo-form__btn
- Атрибут aria-label для доступности

#### TodoForm.vue - форма добавления задачи

```html
<template>
  <ul class="todo-list">
    <!-- Задачи будут добавляться динамически -->
  </ul>
</template>

```

**Особенности:**

- Список (`<ul>`) как контейнер для элементов
- Будет заполняться через `v-for` после подключения данных

#### TodoItem.vue - отдельная задача

```html
<template>
  <div class="todo-item">
    <label class="todo-item__label">
      <input type="checkbox" class="todo-item__checkbox">
      <span class="todo-item__text">Пример задачи</span>
    </label>
    <button class="todo-item__btn" aria-label="Удалить">×</button>
  </div>
</template>

```

**Особенности:**

- Чекбокс для отметки выполнения
- Кнопка удаления
- Полный контроль над стилями (можно скрыть нативный чекбокс)

### 3. Стили по БЭМ

Создаём папку src/assets/scss/components/ и файлы:

src/assets/scss/components/
├── _todo-form.scss
├──_todo-list.scss
└── _todo-item.scss

Подключаем их в src/assets/scss/style.scss:

### 4. Подключение компонентов в App.vue

Обновляем шаблон App.vue:

```html
<template>
  <main class="main">
    <div class="container">
      <h1>Список дел</h1>
      <TodoForm />
      <TodoList />
    </div>
  </main>
</template>

<script setup>
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'
</script>

```

### 5. Проверка результата

- [X] Все компоненты созданы
- [X] Разметка соответствует БЭМ
- [X] Стили подключены
- [X] Нет ошибок в консоли
- [X] Нет ошибок в консоли

## 🚀 Второй этап: Реактивность и управление состоянием

Цель второго этапа - оживить приложение:

- Сделать форму добавления задачи рабочей
- Отобразить список задач
- Реализовать отметку выполнения и удаление
- Научиться передавать данные между компонентами

---

### 1. Добавляем реактивное состояние в `App.vue`

Используем `ref` из Vue для создания реактивного массива задач.

```html
<script setup>
import { ref } from 'vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

// Реактивный массив задач
const todos = ref([
  { id: 1, text: 'Изучить Vue', done: true },
  { id: 2, text: 'Создать TODO-приложение', done: false }
])
</script>

```

**Пояснение:**

- ref() делает переменную реактивной - при изменении Vue перерисует интерфейс
- Каждая задача имеет: id, text, done
- Используется `<script setup>` - все переменные автоматически доступны в шаблоне

### 2. Передаём данные в TodoList

Обновляем App.vue , чтобы передать todos в компонент:

`<TodoList :todos="todos" />`

Теперь TodoList получает список задач как пропс.

### 3. Реализуем динамический рендеринг в TodoList.vue

Обновляем TodoList.vue, чтобы отображать задачи через `v-for`.

```html

<script setup>
import TodoItem from './TodoItem.vue' // Не забудь импортировать TodoItem
defineProps({
  todos: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <ul class="todo-list">
    <li
      v-for="todo in todos"
      :key="todo.id"
      class="todo-list__item"
      :class="{ 'todo-list__item--done': todo.done }"
    >
      <TodoItem :todo="todo" />
    </li>
  </ul>
</template>

```

**Особенности:**

- `v-for` - рендерит элементы списка
- `:key` - уникальный ключ для эффективного обновления DOM
- `:class` - динамический класс для стилизации выполненных задач

### 4. Отображаем данные в TodoItem.vue

Обновляем TodoItem.vue, чтобы показывать текст задачи.

```html
<script setup>
defineProps({
  todo: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="todo-item">
    <label class="todo-item__label">
      <input type="checkbox" :checked="todo.done" class="todo-item__checkbox">
      <span class="todo-item__text">{{ todo.text }}</span>
    </label>
    <button class="todo-item__btn" aria-label="Удалить">×</button>
  </div>
</template>

```

Теперь каждая задача отображается с правильным текстом и статусом.

### 5. Делаем форму интерактивной

Шаг 1: Добавляем v-model в TodoForm.vue

```html
<script setup>
import { ref } from 'vue'

const inputValue = ref('')
</script>

<template>
  <form class="todo-form" @submit.prevent="$emit('submit', inputValue)">
    <input
      v-model="inputValue"
      type="text"
      placeholder="Что нужно сделать?"
      class="todo-form__field"
      aria-label="Новая задача"
    >
    <button type="submit" class="todo-form__btn">Добавить</button>
  </form>
</template>

```

**Что делает:**

- `v-model` связывает поле ввода с `inputValue`
- `@submit.prevent` - предотвращает перезагрузку страницы
- `$emit('submit', ...)` - отправляет текст родителю

### 6. Обрабатываем события в App.vue

Добавляем логику для добавления, удаления и переключения статуса

```html
<script setup>
// ... остальной код

// Генерация уникального ID
const generateId = () => Date.now()

// Добавление новой задачи
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

// Удаление задачи
const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}
</script>

```

Подключаем события в шаблоне:

```html
<template>
  <main class="main">
    <div class="container">
      <h1>Список дел</h1>
      <TodoForm @submit="addTodo" />
      <TodoList
        :todos="todos"
        @toggle="toggleTodo"
        @remove="removeTodo"
      />
    </div>
  </main>
</template>

```

### 7. Добавляем события в TodoItem.vue

Теперь TodoItem может сообщать о действиях пользователя.

```html
<script setup>
defineProps({
  todo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle', 'remove'])
</script>

<template>
  <div class="todo-item">
    <label class="todo-item__label">
      <input
        type="checkbox"
        :checked="todo.done"
        @change="$emit('toggle', todo.id)"
        class="todo-item__checkbox"
      >
      <span class="todo-item__text">{{ todo.text }}</span>
    </label>
    <button
      @click="$emit('remove', todo.id)"
      class="todo-item__btn"
      aria-label="Удалить"
    >
      ×
    </button>
  </div>
</template>

```

### 8. Проверка результата

**Что должно работать:**

- [X] При запуске отображаются начальные задачи
- [X] Можно добавить новую задачу
- [X] Можно отметить задачу как выполненную
- [X] Можно удалить задачу
- [X] Нет ошибок в консоли

## 🚀 Третий этап: Сохранение данных в `localStorage`

Цель третьего этапа - сделать приложение **устойчивым к перезагрузке**:

- Сохранять задачи между сессиями
- Автоматически загружать данные при запуске
- Использовать реактивность Vue для отслеживания изменений

---

### 1. Импортируем `watch` из Vue

Нам нужно отслеживать изменения списка задач.  
Обновляем импорт в `App.vue`:

```js
<script setup>
import { ref, watch } from 'vue'
// ... остальные импорты
</script>
```

### 2. Читаем задачи из localStorage при старте

Заменяем инициализацию todos:

```js
const todos = ref(
  JSON.parse(localStorage.getItem('todos')) || [
    { id: 1, text: 'Изучить Vue', done: true },
    { id: 2, text: 'Создать TODO-приложение', done: false }
  ]
)

```

**Что делает:**

- `localStorage.getItem('todos')` - пытается получить сохранённые задачи

- `JSON.parse()` - преобразует строку в объект

- Если данных нет - используется начальный массив

### 3. Сохраняем изменения при каждом обновлении

Добавляем наблюдатель:

```js
watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos))
}, { deep: true })


```

**Пояснение:**

- `watch` отслеживает изменения `todos`

- `{ deep: true }` - необходимо, так как мы меняем свойства внутри объектов `(todo.done)`

- `JSON.stringify` - преобразует массив в строку (формат, который поддерживает localStorage)

### 4. Проверка результата

Что должно работать:

- [x] При запуске отображаются ранее добавленные задачи

- [x] После добавления/удаления/изменения задач - они сохраняются

- [x] При перезагрузке страницы - список остаётся

- [x] Нет ошибок в консоли

## Четвёртый этап: Фильтрация задач

Цель — позволить пользователю фильтровать задачи: все, активные, выполненные.

### 1. Активный фильтр

```js
const activeFilter = ref('all')
```

### 2. Вычисляемый список

```js
const filteredTodos = computed(() => {
  if (activeFilter.value === 'active') {
    return todos.value.filter(todo => !todo.done)
  }
  if (activeFilter.value === 'done') {
    return todos.value.filter(todo => todo.done)
  }
  return todos.value
})
```

### 3. Компонент TodoFilters.vue

Вынесен в отдельный компонент для чистоты кода.

```js
defineProps({
  activeFilter: {
    type: String,
    required: true,
    default: 'all',
    validator: (value) => ['all', 'active', 'done'].includes(value)
  }
})

```

- default: 'all' — защита от undefined

- validator — проверка допустимых значений

В App.vue:

```js
const setFilter = (filter) => {
  activeFilter.value = filter
}

```

**Проверка:**

- [x]  Фильтры работают

- [x] Нет ошибок в консоли

- [x] HMR не ломает состояние благодаря default

## 🚀 Пятый этап: Счётчик активных задач

Цель — показать пользователю, сколько задач осталось выполнить, **без избыточной анимации**, но с акцентом на важность информации.

### 1. Вычисляемое свойство

```js
const activeCount = computed(() => {
  return todos.value.filter(todo => !todo.done).length
})

```

- Использует computed для реактивного подсчёта

- Фильтрует задачи по done: false

- Автоматически обновляется при любых изменениях

### 2. Вывод в шаблоне

```html

<p class="todo-count">
  Осталось: <span class="todo-count__value">{{ activeCount }}</span> задач
</p>

```


## Компонент: TodoControls

Назначение 

- Объединяет элементы управления над списком задач:

- Отображает количество активных задач

- Показывает кнопку "Очистить завершённые" только при наличии выполненных задач

```html
<div class="todo-controls">
  <p class="todo-count">Осталось: <span class="todo-count__value">{{ activeCount }}</span> задач</p>
  <button v-if="hasCompleted" class="todo-controls__clear-btn" @click="$emit('clear-done')">
    Очистить завершённые
  </button>
</div>

```
