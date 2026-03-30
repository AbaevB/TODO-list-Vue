<!-- src/components/TodoItem.vue -->
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
  <!-- Чекбокс -->
  <input
    :id="`todo-checkbox-${todo.id}`"
    type="checkbox"
    :checked="todo.done"
    @change="$emit('toggle', todo.id)"
    class="todo-item__checkbox visually-hidden"
  >

  <!-- Метка — привязана через for -->
  <label 
    :for="`todo-checkbox-${todo.id}`" 
    class="todo-item__label"
  >
    <span class="todo-item__text">{{ todo.text }}</span>
  </label>

  <!-- Кнопка удаления -->
  <button @click="$emit('remove', todo.id)" class="todo-item__btn">
    &#x2718;
  </button>
</div>
</template>
