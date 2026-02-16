<script setup>
defineProps(['newAPI'])
const emit = defineEmits(['deleteApi'])
function handleDeleteAPI() {
  emit('deleteApi')
}
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
    <button v-on:click="handleDeleteAPI">Delete</button>
  </div>
</template>

<style lang="css" scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  /* border: 1px solid white; */
  width: 400px;
  background-color: #343333;
  border-radius: 10px;
  padding: 5px;
}

.title-container {
  display: flex;
  justify-content: space-between;
  width: 220px;
  text-align: center;
  margin: auto;
}

button {
  background-color: #282828;
  padding: 0.3rem;
  color: white;

  width: 100px;
  border-radius: 10px;
  border: 1px solid #282828;
  font-weight: 400;
  font-family: 'Nunito', Tahoma, Geneva, Verdana, sans-serif;
}
button:hover {
  border: 1px solid #4077d1;
  cursor: pointer;
  transition: 0.3s;
  /* color: #4077d1; */
}
</style>
