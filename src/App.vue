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
  height: 100vh;
  width: 30rem;
  align-items: center;
  margin: auto;
  padding: 1;
  /* border: 1px solid black; */
  /*background-color: black;*/
}
.main-wrapper {
}
</style>
