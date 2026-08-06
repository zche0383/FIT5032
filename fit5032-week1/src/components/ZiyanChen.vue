<script setup>
defineProps({
  title: String,
})
</script>

<template>
  <div class="my-component">
    <h2>{{ title || 'my component' }}</h2>
    <p>This is a new component!</p>
  </div>
</template>

<style scoped>
.my-component {
  border: 2px solid #42b983;
  padding: 1rem;
  margin: 1rem 0;
  border-radius: 8px;
}
</style>