<template>
  <div class="app-container">
    <TopBar />
    <div class="main-content">
      <HomePage v-if="currentPage === 1" @navigate="handleNavigate" />
      <KnowledgePage v-if="currentPage === 2" @navigate="handleNavigate" />
      <PracticePage v-if="currentPage === 3" @navigate="handleNavigate" />
      <RunCodePage v-if="currentPage === 4" @navigate="handleNavigate" />
      <ChartPage v-if="currentPage === 5" @navigate="handleNavigate" />
    </div>
  </div>
</template>

<script setup>
import { ref, provide } from 'vue'
import TopBar from './components/TopBar.vue'
import HomePage from './pages/HomePage.vue'
import KnowledgePage from './pages/KnowledgePage.vue'
import PracticePage from './pages/PracticePage.vue'
import RunCodePage from './pages/RunCodePage.vue'
import ChartPage from './pages/ChartPage.vue'

const currentPage = ref(1)
const userCode = ref([])

const setUserCode = (code) => {
  userCode.value = code
}

provide('userCode', userCode)
provide('setUserCode', setUserCode)

const handleNavigate = (to) => {
  currentPage.value = to
}
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  background-color: #f5f5f7;
}

.main-content {
  max-width: 1000px;
  margin: 0 auto;
}
</style>
