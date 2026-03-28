<script setup>

defineProps({
  msg: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <div class="greetings">
    <h1 class="main-title">{{ msg }}</h1>
    <h2 class="main-subtitle">
      Учебный проект планировщика задач
    </h2>
  </div>
</template>

<style scoped lang="scss">
@import '../assets/scss/style.scss';
.main-title{
  font-size: 3rem;
  font-weight: 200;
  color: $color-white;
}
.main-subtitle{
  font-size: 1rem;
  font-weight: 300;
  text-transform: uppercase;
  color: $color-white;
}
</style>
