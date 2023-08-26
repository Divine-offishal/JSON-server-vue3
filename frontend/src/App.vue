<script setup>
import axios from 'axios'
import { onMounted, reactive } from 'vue'

const fetchParams = reactive({
  data: [],
  error: null,
  isLoading: false
})

const fetchData = () => {
  fetchParams.isLoading = true
  axios.get('http://localhost:4000/meals')
  .then((response) => {
    fetchParams.data = response.data
    fetchParams.isLoading = false
  })
  .catch((error) => {
    fetchParams.isLoading = false
    fetchParams.error = error.message
  })
}

onMounted(() => {
  fetchData()
})
</script>

<template>

  <div v-if="fetchParams.isLoading">
    <h1 class="loading-text">Loading....</h1>
  </div>

  <div v-else-if="fetchParams.error">
    <h1>{{ error }}</h1>
  </div>

  <div v-else-if="fetchParams.data">
    <h1>Meals</h1>
    <div v-for="item in fetchParams.data" class="dataCard">
      <h1>Name: {{ item.title }}</h1>
      <h2>Category: {{ item.category }}</h2>
      <h2>description: {{ item.description }}</h2>
      <h3>Price: ${{ item.price }}</h3>
    </div>
  </div>

</template>

<style>
.loading-text {
  text-align: center;
}

.dataCard {
  text-align: center;
  background-color: cornflowerblue;
  height: auto;
  width: auto;
  padding: 10px;
  margin-top: 30px;
}
</style>

