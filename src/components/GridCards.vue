<script setup>
defineProps(['newAPI'])
</script>

<template>
  <div class="wrapper">
    <div class="title-container">
      <h1>Title: {{ newAPI.title }}</h1>
      <span>{{ newAPI.emoji }}</span>
    </div>
    <div class="data-container">
      <p><strong>Description:</strong> {{ newAPI.description }}</p>
      <div>
        <strong>Source: </strong
        ><a :href="`${newAPI.documentation}`" target="_blank">{{ newAPI.title }}</a>
      </div>
      <span><strong>Health:</strong> {{ newAPI.health }}</span>
    </div>
  </div>
</template>

<style lang="css" scoped></style>
