<!-- src/components/TodoForm.vue -->
<script setup>

</script>
<template>
  <form class="todo-form">
    <input
    v-model="inputValue"
    type="text"
    placeholder="Введите название задачи"
    class="todo-form__field"
    aria-label="Новая задача"
    >
    <button type="submit" class="btn todo-form__btn">
        Добавить
    </button>
  </form>  
</template>