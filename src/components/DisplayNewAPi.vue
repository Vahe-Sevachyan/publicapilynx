<template>
  <div class="wrapper">
    <div class="title-container">
      <h1>{{ title }}</h1>
      <span>{{ emoji }}</span>
    </div>
    <div class="data-container">
      <strong>Description:</strong>
      <p class="description">{{ description }}</p>
      <div class="source">
        <strong>Docs: </strong><a :href="documentation" target="_blank">{{ title }}</a>
      </div>
      <span class="health"><strong>Health:</strong> {{ health }}</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  title: String,
  documentation: String,
  description: String,
  emoji: String,
  health: Number,
})
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 400px;
  font-family: 'Nunito', Tahoma, Geneva, Verdana, sans-serif;
}
strong {
  color: #618aca;
}
h1 {
  font-size: 28px;
}
a {
  text-decoration: underline;
}
a:hover {
  color: gray;
  transition: 0.9s;
}
.data-container .description {
  margin-bottom: 0.8rem;
}
.health,
.source {
  margin-bottom: 0.5rem;
}

p {
  text-align: center;
  margin-bottom: 0.5rem;
}
.title-container {
  display: flex;
  align-items: center;
}
.title-container strong {
  margin-right: 0.3rem;
}
.data-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
