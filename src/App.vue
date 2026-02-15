<script setup>
import FetchData from './FetchData.vue'
</script>

<template>
  <div class="main-wrapper f">
    <div class="wrapper">
      <FetchData />
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: auto;
  padding: 1;
}
</style>
