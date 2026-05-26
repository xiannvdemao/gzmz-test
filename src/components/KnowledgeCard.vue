<template>
  <div class="knowledge-card">
    <div class="knowledge-header" @click="toggle">
      <div class="number-badge">{{ index + 1 }}</div>
      <span class="knowledge-title">{{ title }}</span>
      <span class="knowledge-arrow" :class="{ open: isOpen }">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </span>
    </div>
    <div class="knowledge-content" :class="{ open: isOpen }">
      <div class="knowledge-text">
        <p v-for="(para, idx) in description" :key="idx" v-html="para"></p>
      </div>
      <div class="code-block" v-if="code">
        <pre>{{ code }}</pre>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

defineProps({
  index: {
    type: Number,
    required: true
  },
  title: {
    type: String,
    required: true
  },
  description: {
    type: Array,
    required: true
  },
  code: {
    type: String,
    default: ''
  }
})

const isOpen = ref(false)

const toggle = () => {
  isOpen.value = !isOpen.value
}
</script>

<style scoped>
.knowledge-card {
  background: white;
  border: 1px solid rgba(201, 55, 55, 0.15);
  border-radius: 12px;
  margin: 16px 0;
  overflow: hidden;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}

.knowledge-card:hover {
  box-shadow: 0 6px 20px rgba(201, 55, 55, 0.15);
  border-color: rgba(201, 55, 55, 0.3);
  transform: translateY(-2px);
}

.knowledge-header {
  padding: 20px 24px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 16px;
  font-size: 16px;
  font-weight: 600;
  color: var(--color-text-primary);
  transition: background 0.3s ease;
  background: linear-gradient(135deg, rgba(201, 55, 55, 0.03), rgba(255, 255, 255, 1));
}

.knowledge-header:hover {
  background: linear-gradient(135deg, rgba(201, 55, 55, 0.08), rgba(255, 255, 255, 1));
}

.number-badge {
  width: 32px;
  height: 32px;
  min-width: 32px;
  border-radius: 8px;
  background: linear-gradient(135deg, var(--color-primary), #a82c2c);
  color: white;
  font-size: 14px;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 3px 10px rgba(201, 55, 55, 0.3);
}

.knowledge-title {
  flex: 1;
  text-align: left;
  line-height: 1.5;
}

.knowledge-arrow {
  color: var(--color-text-muted);
  transition: transform 0.3s ease, color 0.3s ease;
  flex-shrink: 0;
}

.knowledge-arrow.open {
  transform: rotate(180deg);
  color: var(--color-primary);
}

.knowledge-content {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease, padding 0.4s ease, opacity 0.3s ease;
  padding: 0 24px;
  opacity: 0;
}

.knowledge-content.open {
  max-height: 1200px;
  padding: 0 24px 24px 72px;
  opacity: 1;
}

.knowledge-text {
  color: var(--color-text-secondary);
  line-height: 1.85;
  margin-bottom: 16px;
}

.knowledge-text p {
  margin-bottom: 12px;
}

.knowledge-text p:last-child {
  margin-bottom: 0;
}

.code-block {
  background: linear-gradient(135deg, #1a1a2e, #16213e);
  color: #e0e0e0;
  padding: 20px;
  border-radius: 10px;
  font-family: 'Consolas', 'Monaco', monospace;
  font-size: 13.5px;
  line-height: 1.7;
  overflow-x: auto;
  border: 1px solid rgba(201, 55, 55, 0.1);
  box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.2);
}

.code-block pre {
  margin: 0;
  white-space: pre-wrap;
  word-break: break-all;
}

@media screen and (max-width: 768px) {
  .knowledge-card {
    margin: var(--spacing-md) 0;
    border-radius: var(--radius-md);
  }
  
  .knowledge-header {
    padding: var(--spacing-md) var(--spacing-lg);
    gap: var(--spacing-sm);
    font-size: var(--font-size-sm);
  }
  
  .number-badge {
    width: 28px;
    height: 28px;
    min-width: 28px;
    font-size: 13px;
  }
  
  .knowledge-content.open {
    padding: 0 var(--spacing-lg) var(--spacing-lg) var(--spacing-xl);
    max-height: 1000px;
  }
  
  .knowledge-text {
    font-size: var(--font-size-sm);
    line-height: 1.7;
  }
  
  .code-block {
    padding: var(--spacing-md);
    font-size: 12.5px;
    line-height: 1.6;
  }
}

@media screen and (max-width: 480px) {
  .knowledge-header {
    padding: var(--spacing-sm) var(--spacing-md);
    font-size: var(--font-size-xs);
  }
  
  .number-badge {
    width: 24px;
    height: 24px;
    min-width: 24px;
    font-size: 12px;
    border-radius: 6px;
  }
  
  .knowledge-content.open {
    padding: 0 var(--spacing-md) var(--spacing-md) var(--spacing-lg);
  }
  
  .knowledge-text {
    font-size: var(--font-size-xs);
    line-height: 1.6;
  }
  
  .knowledge-text p {
    margin-bottom: var(--spacing-xs);
  }
  
  .code-block {
    padding: var(--spacing-sm);
    font-size: 11.5px;
  }
}

@media screen and (max-width: 360px) {
  .knowledge-header {
    gap: var(--spacing-xs);
  }
  
  .knowledge-title {
    line-height: 1.4;
  }
  
  .code-block {
    font-size: 11px;
  }
}
</style>
