<!-- src/components/TodoForm.vue -->
<script setup>
import { ref } from 'vue'

const inputValue = ref('')

const emit = defineEmits(['submit'])

const handleSubmit = () => {
  if (inputValue.value.trim()) {
    emit('submit', inputValue.value)
    inputValue.value = '' // очистка после отправки
  }
}
</script>

<template>
  <form class="todo-form" @submit.prevent="handleSubmit">
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



