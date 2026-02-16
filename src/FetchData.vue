<template>
  <div class="nav">
    <h1>Public API Lynx</h1>
    <p class="sub-heading">Discover new API's for your next project</p>
    <p v-if="loading">Loading API's...</p>
    <p v-if="error">{{ error }}</p>
    <button @click="fetchData">New API</button>
  </div>
  <DisplayNewAPi
    :title="newApiData.title"
    :documentation="newApiData.documentation"
    :description="newApiData.description"
    :emoji="newApiData.emoji"
    :health="newApiData.health"
  />
  <div class="button-container">
    <button @click="handleSaveApi">Save</button>
  </div>

  <div id="grid-cards">
    <ApiCard
      class="api-card"
      v-for="(savedList, index) in savedApiDataList"
      :key="savedList.id"
      :newAPI="savedList"
      @deleteApi="handleDeleteApi(index)"
    />
  </div>
</template>

<script setup>
import DisplayNewAPi from './components/DisplayNewAPi.vue'
import { ref, onMounted } from 'vue'
import ApiCard from './components/ApiCard.vue'
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
    health: newApiData.value.health,
  })
}

function handleDeleteApi(index) {
  const confirmDelete = confirm(`Are you sure you want to delete?`)
  if (confirmDelete) {
    if (index !== -1) {
      savedApiDataList.value.splice(index, 1)
    }
  }
}

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

// const localStorage =
</script>

<style lang="css" scoped>
.nav {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  color: #618aca;
}

#grid-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}
button {
  background-color: #282828;
  padding: 0.5rem;
  color: white;
  width: 140px;
  border-radius: 10px;
  border: 1px solid #282828;
  font-weight: 400;
  font-family: 'Nunito', Tahoma, Geneva, Verdana, sans-serif;
}
button:hover {
  border: 1px solid #4077d1;
  cursor: pointer;
  transition: 0.3s;
}
h1 {
  margin-bottom: 1rem;
  color: #4077d1;
  font-size: 45px;
  font-weight: 200;
  font-family: 'Nunito', Tahoma, Geneva, Verdana, sans-serif;
}
.sub-heading {
  margin-bottom: 1rem;
  font-size: 20px;
}
.sub-heading p {
  margin-bottom: 1rem;
}
button {
  margin-bottom: 1rem;
}
</style>
