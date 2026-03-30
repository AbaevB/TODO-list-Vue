<!-- src/components/TodoFilters.vue -->
 <script setup>
// Пропс: текущий активный фильтр
defineProps({
  activeFilter: {
    type: String,
    required: true,
    default: 'all', 
    validator: (value) => ['all', 'active', 'done'].includes(value)
  }
})

// Событие: изменение фильтра
const emit = defineEmits(['filterChange'])

// Удобные методы для кнопок
const setFilter = (filter) => {
  emit('filterChange', filter)
}
</script>

<template>
  <div class="filters">
    <button
      type="button"
      :class="{ active: activeFilter === 'all' }"
      @click="setFilter('all')"
    >
      Все
    </button>
    <button
      type="button"
      :class="{ active: activeFilter === 'active' }"
      @click="setFilter('active')"
    >
      Активные
    </button>
    <button
      type="button"
      :class="{ active: activeFilter === 'done' }"
      @click="setFilter('done')"
    >
      Выполненные
    </button>
  </div>
</template>

