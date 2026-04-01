<!-- src/components/TodoControls.vue -->
<script setup>
defineProps({
  activeCount: {
    type: Number,
    required: true
  },
  hasCompleted: {
    type: Boolean,
    required: true
  }
})

defineEmits(['clear-done'])
</script>

<template>
  <div class="todo-controls">
    <p class="todo-count">
      Осталось: <span class="todo-count__value">{{ activeCount }}</span> задач
    </p>

    <button
      v-if="hasCompleted"
      type="button"
      class="todo-controls__clear-btn"
      @click="$emit('clear-done')"
    >
      Очистить завершённые
    </button>
  </div>
</template>

<style scoped>
.todo-controls {
  margin-top: 1rem;
  text-align: center;
}
</style>
