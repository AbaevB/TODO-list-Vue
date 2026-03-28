<!-- src/components/TodoList.vue -->
 <script setup>
import TodoItem from './TodoItem.vue';

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
    :class="{'todo-list__item--done' : todo.done}"
    >
    <TodoItem 
    :todo="todo"
    @toggle="$emit('toggle', todo.id)"
    @remove="$emit('remove', todo.id)"
    />

    </li>
</ul>
</template>