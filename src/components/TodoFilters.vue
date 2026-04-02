<!-- src/components/TodoFilters.vue -->
<script setup>
defineProps({
  modelValue: {
    type: String,
    required: true,
    default: 'all',
    validator: (value) => ['all', 'active', 'done'].includes(value)
  }
})

defineEmits(['update:modelValue'])
</script>

<template>
  <div class="filters">
    

    <button
      v-for="filter in ['all', 'active', 'done']"
      :key="filter"
      class="filters__btn"
      :class="{ 'filters__btn--active': modelValue === filter }"
      @click="$emit('update:modelValue', filter)"
    >
      {{ filter === 'all' ? 'Все' : filter === 'active' ? 'Активные' : 'Выполненные' }}
    </button>
  </div>
</template>