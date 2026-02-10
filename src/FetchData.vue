<template>
  <div class="nav">
    <h1>Public API Lynx</h1>
    <p class="sub-heading">Find API's for your next project</p>
    <p v-if="loading">Loading API's...</p>
    <p v-if="error">{{ error }}</p>
    <button @click="fetchData">Get New API</button>
  </div>
  <DisplayData
    :title="posts.title"
    :documentation="posts.documentation"
    :description="posts.description"
    :emoji="posts.emoji"
    :health="posts.health"
  />
</template>

<script setup>
import DisplayData from './DisplayData.vue'
import { ref, onMounted } from 'vue'

const posts = ref([])
const error = ref(null)
const loading = ref(true)

const fetchData = () => {
  fetch('https://www.freepublicapis.com/api/random')
    .then((response) => {
      if (!response.status) {
        throw Error('Error when loading data')
      }
      return response.json()
    })
    .then((data) => {
      posts.value = data
      console.log(posts)
      if (posts.value.title.includes('Lucifer')) {
        fetchData()
      }
    })
    .catch((err) => {
      error.value = err.message
    })
    .finally(() => {
      loading.value = false
    })
}
onMounted(() => {
  fetchData()
})
</script>

<style scoped>
.wrapper {
  margin: 1;
}
.nav {
  display: flex;
  flex-direction: column;
}
h1 {
  margin-bottom: 1rem;
}
.sub-heading {
  margin-bottom: 1rem;
}
.sub-heading p {
  margin-bottom: 1rem;
}
button {
  margin-bottom: 1rem;
}
</style>
