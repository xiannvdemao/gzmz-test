<template>
  <div class="code-exercise">
    <div class="code-context">
      <pre>{{ context }}</pre>
    </div>
    <button class="tip-btn" @click="toggleAnswer">
      {{ showAnswer ? '隐藏答案' : '提示 / 答案' }}
    </button>
    <div class="answer-area" :class="{ visible: showAnswer }">
      <pre>{{ answer }}</pre>
    </div>
    <textarea
      :id="textareaId"
      class="code-input"
      :placeholder="placeholder"
      v-model="userInput"
    ></textarea>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  id: {
    type: Number,
    required: true
  },
  context: {
    type: String,
    required: true
  },
  answer: {
    type: String,
    required: true
  },
  placeholder: {
    type: String,
    default: '填写代码'
  }
})

const emit = defineEmits(['input'])

const showAnswer = ref(false)
const userInput = ref('')

const textareaId = `code-input-${props.id}`

const toggleAnswer = () => {
  showAnswer.value = !showAnswer.value
}

const getUserInput = () => {
  return userInput.value
}

defineExpose({ getUserInput })
</script>

<style scoped>
.code-exercise {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-sm);
  margin: var(--spacing-lg) 0;
}

.code-context {
  width: 100%;
  background: var(--color-surface-parchment);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  font-family: 'Consolas', 'Monaco', monospace;
  font-size: 14px;
  color: var(--color-text-primary);
  line-height: 1.6;
  text-align: left;
  white-space: pre-wrap;
}

.code-context pre {
  margin: 0;
}

.tip-btn {
  padding: 6px 16px;
  font-size: 13px;
  font-weight: 500;
  color: white;
  background: #faad14;
  border: none;
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all 0.25s ease;
}

.tip-btn:hover {
  background: #d4940f;
  transform: scale(0.98);
}

.answer-area {
  width: 100%;
  background: #e6f7ff;
  border: 1px solid #91d5ff;
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  font-family: 'Consolas', monospace;
  font-size: 13px;
  color: #1890ff;
  line-height: 1.6;
  display: none;
}

.answer-area.visible {
  display: block;
}

.answer-area pre {
  margin: 0;
  white-space: pre-wrap;
}

.code-input {
  width: 100%;
  height: 80px;
  padding: var(--spacing-md);
  font-size: 15px;
  font-family: 'Consolas', 'Monaco', monospace;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  outline: none;
  background: var(--color-surface-parchment);
  resize: none;
  transition: all 0.25s ease;
}

.code-input:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(201, 55, 55, 0.1);
  background: white;
}

.code-input::placeholder {
  color: var(--color-text-muted);
}
</style>
