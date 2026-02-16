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
      <h1><strong>Title:</strong> {{ newAPI.title }}</h1>
      <span>{{ newAPI.emoji }}</span>
    </div>
    <div class="data-container">
      <div class="discription-container">
        <strong>Description:</strong>
        <p>{{ newAPI.description }}</p>
      </div>

      <div class="link-container">
        <strong>Docs: </strong
        ><a :href="`${newAPI.documentation}`" target="_blank">{{ newAPI.title }}</a>
      </div>
      <div class="health-container">
        <span><strong>Health:</strong> {{ newAPI.health }}</span>
      </div>
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
strong {
  color: #618aca;
}
.discription-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: left;
  margin-bottom: 6px;
}
a {
  text-decoration: underline;
}
a:hover {
  color: gray;
  transition: 0.9s;
}
.discription-container strong {
  margin-bottom: 4px;
}
.title-container {
  display: flex;
  justify-content: space-between;
  padding: 5px;
  text-align: center;
}
.health-container {
  margin-bottom: 6px;
}
.title-container h1 {
  margin-right: 10px;
}
.link-container {
  margin-bottom: 6px;
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
