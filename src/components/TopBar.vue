<template>
  <div class="top-bar">
    <div class="top-bar-inner">
      <div class="top-bar-left">
        <svg class="logo" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M12 2L2 7v10l10 5 10-5V7L12 2z" fill="#c93737"/>
          <path d="M12 6.5l5.5 3-5.5 9-5.5-9L12 6.5z" fill="white"/>
        </svg>
        <span class="top-bar-title">红色数据可视化</span>
      </div>
      <div class="top-bar-right">
        <span class="top-bar-date">{{ currentDate }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const currentDate = computed(() => {
  const now = new Date()
  return `${now.getFullYear()}年${now.getMonth() + 1}月${now.getDate()}日`
})
</script>

<style scoped>
.top-bar {
  background: linear-gradient(90deg, #c93737, #a82c2c);
  padding: 10px 20px;
  box-shadow: 0 2px 10px rgba(201, 55, 55, 0.3);
}

.top-bar-inner {
  max-width: 1000px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.top-bar-left {
  display: flex;
  align-items: center;
  gap: 12px;
}

.logo {
  width: 28px;
  height: 28px;
}

.top-bar-title {
  font-size: 16px;
  font-weight: 600;
  color: white;
  letter-spacing: 0.5px;
}

.top-bar-right {
  color: rgba(255, 255, 255, 0.85);
  font-size: 14px;
}
</style>
