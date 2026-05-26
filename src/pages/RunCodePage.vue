<template>
  <div class="page-container run-page">
    <div class="run-content">
      <div class="loading-spinner">
        <div class="spinner-ring"></div>
        <div class="spinner-dot"></div>
      </div>
      <h2 class="run-title">你的代码正在运行中...</h2>
      <p class="run-subtitle">正在编译 ECharts 可视化组件</p>
      <div class="console-logs">
        <div class="log-item" v-for="(log, index) in logs" :key="index">
          <span class="log-time">[{{ log.time }}]</span>
          <span class="log-text">{{ log.text }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const emit = defineEmits(['navigate'])

const logs = ref([])

const logSequence = [
  '开始编译...',
  '加载 ECharts 库',
  '初始化图表容器',
  '配置数据选项',
  '渲染可视化组件',
  '编译成功！'
]

onMounted(() => {
  let index = 0
  const interval = setInterval(() => {
    if (index < logSequence.length) {
      const now = new Date()
      logs.value.push({
        time: `${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}:${String(now.getSeconds()).padStart(2, '0')}`,
        text: logSequence[index]
      })
      index++
    } else {
      clearInterval(interval)
      setTimeout(() => {
        emit('navigate', 5)
      }, 1200)
    }
  }, 600)
})
</script>

<style scoped>
.run-page {
  min-height: 70vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px;
}

.run-content {
  text-align: center;
  width: 100%;
  max-width: 600px;
}

.loading-spinner {
  position: relative;
  width: 100px;
  height: 100px;
  margin: 0 auto 30px;
}

.spinner-ring {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border: 4px solid #e0e0e0;
  border-top-color: var(--color-primary);
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

.spinner-dot {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 20px;
  height: 20px;
  background: var(--color-primary);
  border-radius: 50%;
  animation: pulse 1.5s ease-in-out infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

@keyframes pulse {
  0%, 100% { opacity: 0.5; transform: translate(-50%, -50%) scale(0.8); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.2); }
}

.run-title {
  font-size: 28px;
  font-weight: 600;
  color: var(--color-surface-black);
  margin-bottom: 12px;
}

.run-subtitle {
  font-size: 16px;
  color: var(--color-text-secondary);
  margin-bottom: 40px;
}

.console-logs {
  background: var(--color-surface-black);
  border-radius: 12px;
  padding: 20px;
  text-align: left;
  max-height: 250px;
  overflow-y: auto;
}

.log-item {
  font-family: 'Consolas', 'Monaco', monospace;
  font-size: 14px;
  color: #50fa7b;
  margin-bottom: 8px;
  opacity: 0;
  animation: fadeIn 0.3s ease forwards;
}

.log-item:last-child {
  margin-bottom: 0;
}

@keyframes fadeIn {
  to { opacity: 1; }
}

.log-time {
  color: #bd93f9;
  margin-right: 10px;
}

@media screen and (max-width: 768px) {
  .run-page {
    padding: var(--spacing-xl);
  }
  
  .run-content {
    max-width: 100%;
  }
  
  .loading-spinner {
    width: 80px;
    height: 80px;
    margin-bottom: var(--spacing-xl);
  }
  
  .run-title {
    font-size: var(--font-size-xl);
    margin-bottom: var(--spacing-sm);
  }
  
  .run-subtitle {
    font-size: var(--font-size-sm);
    margin-bottom: var(--spacing-xl);
  }
  
  .console-logs {
    padding: var(--spacing-lg);
    max-height: 200px;
  }
  
  .log-item {
    font-size: 13px;
    margin-bottom: var(--spacing-xs);
  }
}

@media screen and (max-width: 480px) {
  .run-page {
    padding: var(--spacing-lg);
  }
  
  .loading-spinner {
    width: 64px;
    height: 64px;
  }
  
  .spinner-ring {
    border-width: 3px;
  }
  
  .spinner-dot {
    width: 16px;
    height: 16px;
  }
  
  .run-title {
    font-size: var(--font-size-lg);
  }
  
  .run-subtitle {
    font-size: var(--font-size-xs);
  }
  
  .console-logs {
    padding: var(--spacing-md);
    max-height: 160px;
  }
  
  .log-item {
    font-size: 12px;
  }
}

@media screen and (max-width: 360px) {
  .loading-spinner {
    width: 56px;
    height: 56px;
  }
  
  .run-title {
    font-size: var(--font-size-base);
  }
  
  .log-item {
    font-size: 11px;
  }
}
</style>
