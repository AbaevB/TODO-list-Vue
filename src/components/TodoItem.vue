<!-- src/components/TodoItem.vue -->
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
            <input 
            type="checkbox" 
            :checked="todo.done"
            @change="$emit('toggle')"
            class="todo-item__checkbox"
            >
            <span class="todo-item__text">
                {{ todo.text }}
            </span>
        </label>

        <button 
            @click="$emit('remove')"
            class="todo-item__btn"
            :aria-label="`Удалить задачу: ${todo.text}`"
        >
        ×
        </button>
    </div>
</template>