<template>
  <div class="nav">
    <h1>Public API Lynx</h1>
    <p class="sub-heading">Find API's for your next project</p>
    <p v-if="loading">Loading API's...</p>
    <p v-if="error">{{ error }}</p>
    <button @click="fetchData">Get New API</button>
  </div>
  <DisplayNewAPi
    :title="newApiData.title"
    :documentation="newApiData.documentation"
    :description="newApiData.description"
    :emoji="newApiData.emoji"
    :health="newApiData.health"
  />
  <button v-on:click="handleSaveApi">Save</button>
  <div class="card-grid">
    <GridCards v-for="savedList in savedApiDataList" :key="savedList.id" :newAPI="savedList" />
  </div>
</template>

<script setup>
import DisplayNewAPi from './components/DisplayNewAPi.vue'
import { ref, onMounted } from 'vue'
import GridCards from './components/GridCards.vue'
const newApiData = ref([])
const savedApiDataList = ref([])
const error = ref(null)
const loading = ref(true)

function handleSaveApi() {
  savedApiDataList.value.push({
    title: newApiData.value.title,
    id: newApiData.value.id,
    emoji: newApiData.value.emoji,
    description: newApiData.value.description,
    documentation: newApiData.value.documentation,
  })
  // console.log(savedApiDataList.value.title)
}

// console.log(newApiData.value)
const fetchData = () => {
  fetch('https://www.freepublicapis.com/api/random')
    .then((response) => {
      if (!response.status) {
        throw Error('Error when loading data')
      }
      return response.json()
    })
    .then((data) => {
      newApiData.value = data
      // console.log(newApiData)
      if (newApiData.value.title.includes('Lucifer')) {
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
