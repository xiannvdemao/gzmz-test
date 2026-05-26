<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$emit('navigate', 2)">←</button>
      <div class="header-content">
        <h1 class="page-title">代码编程实践</h1>
        <p class="page-subtitle">填写代码，运行你的可视化作品</p>
        <div class="progress-info">
          <span class="progress-text">{{ completedCount }} / {{ exercises.length }}</span>
          <span class="progress-label">已完成</span>
        </div>
      </div>
    </div>
    <div class="divider"></div>
    <div class="page-content">
      <div class="exercise-grid">
        <div class="left-panel">
          <div class="exercise-item" v-for="(item, index) in exercises" :key="index">
            <div class="exercise-header">
              <div class="header-left">
                <div class="exercise-number">0{{ index + 1 }}</div>
                <div class="header-info">
                  <span class="exercise-status" :class="isCorrect(index) ? 'correct' : (hasValue(index) ? 'incomplete' : 'empty')">
                    {{ getStatusText(index) }}
                  </span>
                </div>
              </div>
            </div>
            <div class="code-context">
              <div class="context-label">📝 代码上下文</div>
              <pre>{{ item.context }}</pre>
            </div>
            <div class="input-area">
              <label class="input-label">✏️ 你的代码</label>
              <textarea
                class="code-input"
                v-model="userAnswers[index]"
                :placeholder="'请填写 ' + item.lines + ' 行代码'"
                @input="onInputChange"
              ></textarea>
            </div>
            <button class="tip-btn" @click="toggleAnswer(index)">
              {{ showAnswers[index] ? '隐藏答案' : '💡 查看答案'
              }}
            </button>
            <div class="answer-area" v-if="showAnswers[index]">
              <div class="answer-header">参考代码</div>
              <pre>{{ item.answer }}</pre>
            </div>
          </div>
        </div>
        <div class="right-panel">
          <div class="progress-card">
            <div class="progress-header">
              <div class="progress-icon">📊</div>
              <div class="progress-info">
                <h3>实践进度</h3>
                <span class="progress-text">{{ completedCount }} / {{ exercises.length }}</span>
              </div>
            </div>
            <div class="progress-bar-wrapper">
              <div class="progress-bar-bg"></div>
              <div class="progress-fill" :style="{ width: progressPercent + '%' }"></div>
            </div>
            <div class="progress-percent">{{ Math.round(progressPercent) }}%</div>
          </div>
          
          <div class="tips-card">
            <h4>💡 实践提示</h4>
            <ul>
              <li>先学习第 1-6 章知识</li>
              <li>仔细阅读知识点后再填写代码</li>
              <li>可以查看答案后自己再写一遍</li>
              <li>完成后点击运行看看效果</li>
            </ul>
          </div>
          
          <div class="quick-actions">
            <button class="btn-secondary" @click="resetAnswers">
              {{ showAnswers.some(v => v) ? '收起所有答案' : '展开所有答案' }}
            </button>
          </div>
          
          <button 
            class="btn-primary submit-btn" 
            :class="{ disabled: !allFilled }"
            :disabled="!allFilled"
            @click="submitCode"
          >
            <span class="btn-text">🚀 运行代码</span>
            <span class="btn-sub">查看可视化成果</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, inject } from 'vue'

const emit = defineEmits(['navigate'])
const setUserCode = inject('setUserCode')

const userAnswers = ref(['', '', '', '', ''])
const showAnswers = ref([false, false, false, false, false])

const exercises = [
  {
    lines: 2,
    context: `// 1. 获取延安GDP红色数据
async function getYanAnGDP() {
  const res = _______; // 请求延安GDP数据
  const data = _______; // 解析响应数据
  return data;
}`,
    answer: `await fetch('/api/yangan-gdp');
await res.json();`,
    correctAnswer: 'await fetch("/api/yangan-gdp");await res.json();'
  },
  {
    lines: 2,
    context: `// 2. 清洗红色GDP数据
function cleanData(rawData) {
  const cleanItems = rawData.map(item => {
    return {
      year: _______, // 转换年份为整数
      gdp: _______   // 转换GDP为浮点数
    };
  });
  return cleanItems.filter(Boolean);
}`,
    answer: `parseInt(item.year);
parseFloat(item.gdp);`,
    correctAnswer: 'parseInt(item.year);parseFloat(item.gdp);'
  },
  {
    lines: 2,
    context: `// 3. 初始化图表 + 异步加载数据
const chartDom = document.getElementById('chart');
const myChart = echarts.init(chartDom);
_______; // 加载远程数据
_______; // 渲染图表配置`,
    answer: `loadData();
myChart.setOption(option);`,
    correctAnswer: 'loadData();myChart.setOption(option);'
  },
  {
    lines: 2,
    context: `// 4. 异步请求数据
async function fetchData(){
  const res = _______;
  const data = _______;
  return data;
}`,
    answer: `await fetch("/api/chart");
await res.json();`,
    correctAnswer: 'await fetch("/api/chart");await res.json();'
  },
  {
    lines: 2,
    context: `// 5. 响应式图表适配
window.addEventListener('resize', ()=>{
  _______; // 防抖
  _______; // 重绘图表
});`,
    answer: `clearTimeout(timer);
myChart.resize();`,
    correctAnswer: 'clearTimeout(timer);myChart.resize();'
  }
]

const getNormalized = (str) => {
  return str.replace(/\s|['";]/g, '').toLowerCase()
}

const isCorrect = (index) => {
  if (!userAnswers.value[index]) return false
  return getNormalized(userAnswers.value[index]) === getNormalized(exercises[index].correctAnswer)
}

const hasValue = (index) => {
  return userAnswers.value[index].trim().length > 0
}

const getStatusText = (index) => {
  if (isCorrect(index)) return '✅ 正确'
  if (hasValue(index)) return '⏳ 已填写'
  return '📝 未填写'
}

const completedCount = computed(() => {
  return userAnswers.value.filter((ans, i) => isCorrect(i)).length
})

const progressPercent = computed(() => {
  return (completedCount.value / exercises.length) * 100
})

const allFilled = computed(() => {
  return userAnswers.value.every(ans => ans.trim().length > 0)
})

const toggleAnswer = (index) => {
  showAnswers.value[index] = !showAnswers.value[index]
}

const resetAnswers = () => {
  const hasAnyOpen = showAnswers.value.some(v => v)
  if (hasAnyOpen) {
    showAnswers.value = showAnswers.value.map(() => false)
  } else {
    showAnswers.value = showAnswers.value.map(() => true)
  }
}

const onInputChange = () => {
  // 记录用户输入，用于后续展示
}

const submitCode = () => {
  if (!allFilled.value) return
  
  // 保存用户代码
  setUserCode && setUserCode(userAnswers.value)
  
  // 跳转到运行页面
  emit('navigate', 4)
}
</script>

<style scoped>
.page-header {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 32px;
}

.header-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.progress-info {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 4px;
}

.progress-info .progress-text {
  font-size: 16px;
  font-weight: 700;
  color: var(--color-primary);
}

.progress-info .progress-label {
  font-size: 14px;
  color: var(--color-text-secondary);
}

.exercise-grid {
  display: grid;
  grid-template-columns: 1fr 360px;
  gap: 28px;
}

.left-panel {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.exercise-item {
  background: white;
  border: 1px solid rgba(201, 55, 55, 0.12);
  border-radius: 16px;
  padding: 24px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.04);
  transition: all 0.3s ease;
}

.exercise-item:hover {
  border-color: rgba(201, 55, 55, 0.25);
  box-shadow: 0 6px 24px rgba(201, 55, 55, 0.1);
}

.exercise-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 16px;
}

.exercise-number {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: linear-gradient(135deg, var(--color-primary), #a82c2c);
  color: white;
  font-size: 18px;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 12px rgba(201, 55, 55, 0.3);
}

.header-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.exercise-status {
  font-size: 13px;
  font-weight: 600;
  padding: 6px 14px;
  border-radius: 20px;
  display: inline-block;
}

.exercise-status.empty {
  background: #f0f0f0;
  color: #888;
}

.exercise-status.incomplete {
  background: #fff3e0;
  color: #f57c00;
}

.exercise-status.correct {
  background: linear-gradient(135deg, #e8f5e9, #c8e6c9);
  color: #2e7d32;
  box-shadow: 0 2px 8px rgba(46, 125, 50, 0.15);
}

.code-context {
  background: linear-gradient(135deg, #fafafa, #f5f5f7);
  border: 1px solid rgba(201, 55, 55, 0.08);
  border-radius: 12px;
  padding: 20px;
  font-family: 'Consolas', 'Monaco', monospace;
  font-size: 13.5px;
  color: var(--color-text-primary);
  line-height: 1.7;
  margin-bottom: 16px;
}

.context-label {
  font-size: 12px;
  font-weight: 600;
  color: var(--color-primary);
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.code-context pre {
  margin: 0;
  white-space: pre-wrap;
}

.input-area {
  margin-bottom: 12px;
}

.input-label {
  font-size: 12px;
  font-weight: 600;
  color: var(--color-primary);
  margin-bottom: 8px;
  display: block;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.code-input {
  width: 100%;
  min-height: 90px;
  padding: 18px;
  font-size: 15px;
  font-family: 'Consolas', 'Monaco', monospace;
  border: 2px solid rgba(201, 55, 55, 0.15);
  border-radius: 12px;
  outline: none;
  background: white;
  resize: vertical;
  transition: all 0.3s ease;
  box-shadow: inset 0 1px 4px rgba(0, 0, 0, 0.04);
}

.code-input:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 5px rgba(201, 55, 55, 0.1), inset 0 1px 4px rgba(0, 0, 0, 0.04);
}

.tip-btn {
  margin-top: 8px;
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 600;
  color: var(--color-primary);
  background: rgba(201, 55, 55, 0.06);
  border: 1px solid rgba(201, 55, 55, 0.2);
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tip-btn:hover {
  background: rgba(201, 55, 55, 0.12);
  transform: translateY(-1px);
}

.answer-area {
  margin-top: 16px;
  background: linear-gradient(135deg, #e8f4fd, #d1eafd);
  border: 1px solid rgba(33, 150, 243, 0.3);
  border-radius: 12px;
  padding: 18px;
  font-family: 'Consolas', monospace;
  font-size: 14px;
  color: #0d47a1;
  box-shadow: inset 0 2px 8px rgba(33, 150, 243, 0.1);
}

.answer-header {
  font-size: 12px;
  font-weight: 700;
  color: #1565c0;
  margin-bottom: 10px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.answer-area pre {
  margin: 0;
  white-space: pre-wrap;
}

.right-panel {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.progress-card {
  background: white;
  border: 1px solid rgba(201, 55, 55, 0.12);
  border-radius: 16px;
  padding: 24px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.04);
}

.progress-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 20px;
}

.progress-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: linear-gradient(135deg, rgba(201, 55, 55, 0.1), rgba(201, 55, 55, 0.05));
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
}

.progress-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.progress-info h3 {
  font-size: 17px;
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0;
}

.progress-info .progress-text {
  font-size: 15px;
  font-weight: 700;
  color: var(--color-primary);
}

.progress-bar-wrapper {
  position: relative;
  height: 12px;
  margin-bottom: 12px;
}

.progress-bar-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #f0f0f0;
  border-radius: 6px;
}

.progress-fill {
  position: relative;
  height: 100%;
  background: linear-gradient(90deg, var(--color-primary), #e57373);
  border-radius: 6px;
  transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 8px rgba(201, 55, 55, 0.3);
}

.progress-percent {
  text-align: right;
  font-size: 14px;
  font-weight: 700;
  color: var(--color-primary);
}

.tips-card {
  background: white;
  border: 1px solid rgba(201, 55, 55, 0.12);
  border-radius: 16px;
  padding: 24px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.04);
}

.tips-card h4 {
  font-size: 16px;
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0 0 16px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.tips-card ul {
  margin: 0;
  padding-left: 20px;
}

.tips-card li {
  font-size: 14px;
  color: var(--color-text-secondary);
  line-height: 2;
}

.quick-actions {
  margin-bottom: 8px;
}

.btn-secondary {
  width: 100%;
  padding: 12px 20px;
  font-size: 14px;
  font-weight: 600;
  color: var(--color-primary);
  background: rgba(201, 55, 55, 0.06);
  border: 1px solid rgba(201, 55, 55, 0.2);
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-secondary:hover {
  background: rgba(201, 55, 55, 0.12);
  transform: translateY(-1px);
}

.submit-btn {
  margin-top: auto;
  height: auto;
  padding: 18px 24px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.submit-btn.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-text {
  font-size: 18px;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.btn-sub {
  font-size: 13px;
  font-weight: 500;
  opacity: 0.9;
}

@media screen and (max-width: 1000px) {
  .exercise-grid {
    grid-template-columns: 1fr;
  }
  
  .right-panel {
    order: -1;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 16px;
  }
  
  .progress-card, .tips-card {
    grid-column: span 1;
  }
  
  .quick-actions, .submit-btn {
    grid-column: span 2;
  }
  
  .submit-btn {
    margin-top: 0;
  }
}

@media screen and (max-width: 768px) {
  .page-header {
    padding: var(--spacing-xl);
    flex-wrap: wrap;
  }
  
  .header-content {
    flex: 1;
    min-width: 200px;
  }
  
  .progress-info {
    margin-top: 4px;
  }
  
  .progress-info .progress-text {
    font-size: 14px;
  }
  
  .progress-info .progress-label {
    font-size: 12px;
  }
  
  .exercise-item {
    padding: var(--spacing-lg);
  }
  
  .exercise-header {
    margin-bottom: var(--spacing-md);
  }
  
  .header-left {
    gap: var(--spacing-sm);
  }
  
  .exercise-number {
    width: 40px;
    height: 40px;
    font-size: 16px;
  }
  
  .exercise-status {
    font-size: 12px;
    padding: 4px 12px;
  }
  
  .code-context {
    padding: var(--spacing-md);
    font-size: 12.5px;
    margin-bottom: var(--spacing-sm);
  }
  
  .code-input {
    min-height: 72px;
    padding: 14px;
    font-size: 13px;
  }
  
  .tip-btn {
    padding: 8px 16px;
    font-size: 13px;
  }
  
  .answer-area {
    padding: 14px;
    font-size: 13px;
  }
  
  .right-panel {
    grid-template-columns: 1fr;
  }
  
  .progress-card, .tips-card, .quick-actions, .submit-btn {
    grid-column: span 1;
  }
  
  .progress-card {
    padding: var(--spacing-lg);
  }
  
  .progress-header {
    margin-bottom: var(--spacing-md);
  }
  
  .progress-icon {
    width: 40px;
    height: 40px;
    font-size: 20px;
  }
  
  .progress-info h3 {
    font-size: 15px;
  }
  
  .progress-info .progress-text {
    font-size: 14px;
  }
  
  .progress-bar-wrapper {
    height: 10px;
    margin-bottom: 10px;
  }
  
  .progress-percent {
    font-size: 13px;
  }
  
  .tips-card {
    padding: var(--spacing-lg);
  }
  
  .tips-card h4 {
    font-size: 15px;
    margin-bottom: 12px;
  }
  
  .tips-card li {
    font-size: 13px;
    line-height: 1.8;
  }
  
  .btn-secondary {
    padding: 10px 16px;
    font-size: 13px;
  }
  
  .submit-btn {
    padding: 16px 20px;
  }
  
  .btn-text {
    font-size: 16px;
  }
  
  .btn-sub {
    font-size: 12px;
  }
}

@media screen and (max-width: 480px) {
  .page-header {
    padding: var(--spacing-lg);
    gap: var(--spacing-md);
  }
  
  .page-title {
    font-size: var(--font-size-xl);
  }
  
  .page-subtitle {
    font-size: var(--font-size-sm);
  }
  
  .exercise-item {
    padding: var(--spacing-md);
  }
  
  .exercise-number {
    width: 36px;
    height: 36px;
    font-size: 14px;
    border-radius: 10px;
  }
  
  .exercise-status {
    font-size: 11px;
    padding: 3px 10px;
  }
  
  .code-context {
    padding: var(--spacing-sm);
    font-size: 11.5px;
  }
  
  .context-label {
    font-size: 11px;
  }
  
  .code-input {
    min-height: 64px;
    padding: 12px;
    font-size: 12px;
    border-radius: 10px;
  }
  
  .tip-btn {
    padding: 6px 14px;
    font-size: 12px;
    border-radius: 8px;
  }
  
  .answer-area {
    padding: 12px;
    font-size: 12px;
  }
  
  .progress-card {
    padding: var(--spacing-md);
  }
  
  .progress-icon {
    width: 36px;
    height: 36px;
    font-size: 18px;
  }
  
  .progress-info h3 {
    font-size: 14px;
  }
  
  .tips-card {
    padding: var(--spacing-md);
  }
  
  .tips-card h4 {
    font-size: 14px;
  }
  
  .tips-card li {
    font-size: 12px;
  }
  
  .submit-btn {
    padding: 14px 16px;
  }
  
  .btn-text {
    font-size: 15px;
  }
}

@media screen and (max-width: 360px) {
  .exercise-number {
    width: 32px;
    height: 32px;
    font-size: 13px;
  }
  
  .code-context {
    font-size: 11px;
  }
  
  .code-input {
    font-size: 11px;
  }
}
</style>
