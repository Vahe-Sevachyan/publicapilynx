<template>
  <div class="wrapper">
    <div class="title-container">
      <h1>{{ title }}</h1>
      <span>{{ emoji }}</span>
    </div>
    <div class="data-container">
      <p><strong>Description:</strong> {{ description }}</p>
      <div>
        <strong>Source: </strong><a :href="documentation" target="_blank">{{ title }}</a>
      </div>
      <span><strong>Health:</strong> {{ health }}</span>
    </div>
  </div>
  <!-- <div class="wrapper">
    <div class="title-container">
      <h1>{{ newAPI.title }}</h1>
      <span>{{ newAPI.emoji }}</span>
    </div>
    <div class="data-container">
      <p><strong>Description:</strong> {{ newAPI.description }}</p>
      <div>
        <strong>Source: </strong
        ><a :href="newAPI.documentation" target="_blank">{{ newAPI.title }}</a>
      </div>
      <span><strong>Health:</strong> {{ newAPI.health }}</span>
    </div>
  </div> -->
</template>

<script setup>
// defineProps(['newAPI'])
defineProps({
  title: String,
  documentation: String,
  description: String,
  emoji: String,
  health: Number,
  // posts: Array,
})

// console.log(props.posts)
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  /* border: 1px solid black; */
  width: 30rem;
  padding: 1rem;
}

.title-container {
  display: flex;
  align-items: center;
}

.data-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
