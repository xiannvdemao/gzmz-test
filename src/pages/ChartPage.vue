<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$emit('navigate', 3)">←</button>
      <div class="header-content">
        <h1 class="page-title">🎉 恭喜！你的可视化作品！</h1>
        <p class="page-subtitle">用你刚才写的代码生成的红色数据可视化</p>
      </div>
    </div>
    <div class="divider"></div>
    <div class="page-content">
      <!-- 代码展示 -->
      <div class="code-review-card">
        <div class="review-header">
          <span class="review-title">📝 你的代码</span>
          <span class="review-status">✅ 编译成功</span>
        </div>
        <div class="review-code">
          <div class="code-line" v-for="(line, index) in codeLines" :key="index">
            <span class="line-number">{{ String(index + 1).padStart(2, '0') }}</span>
            <span class="line-text" :class="getLineType(line)">{{ line }}</span>
          </div>
        </div>
      </div>

      <!-- 图表展示 -->
      <div class="charts-section">
        <div class="chart-card">
          <div class="chart-label">
            <span class="chart-number">图表 1</span>
            <h3>延安革命老区 GDP 与增长趋势</h3>
          </div>
          <div ref="chart1Ref" class="chart-item"></div>
        </div>

        <div class="chart-card">
          <div class="chart-label">
            <span class="chart-number">图表 2</span>
            <h3>全国党员发展与城乡结构</h3>
          </div>
          <div ref="chart2Ref" class="chart-item"></div>
        </div>

        <div class="chart-card">
          <div class="chart-label">
            <span class="chart-number">图表 3</span>
            <h3>脱贫攻坚成果显著</h3>
          </div>
          <div ref="chart3Ref" class="chart-item"></div>
        </div>
      </div>

      <!-- 总结卡片 -->
      <div class="summary-card">
        <div class="summary-icon">🏆</div>
        <h2>学习完成！</h2>
        <p>你已经掌握了 ECharts 数据可视化的核心技能。</p>
        <div class="summary-stats">
          <div class="stat">
            <div class="stat-value">4</div>
            <div class="stat-label">知识点</div>
          </div>
          <div class="stat">
            <div class="stat-value">40+</div>
            <div class="stat-label">代码行</div>
          </div>
          <div class="stat">
            <div class="stat-value">3</div>
            <div class="stat-label">可视化图表</div>
          </div>
        </div>
        <button class="btn-secondary" @click="restartLearning">再学一遍 🔄</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed, inject } from 'vue'
import * as echarts from 'echarts'

const emit = defineEmits(['navigate'])
const userCode = inject('userCode', [])

const chart1Ref = ref(null)
const chart2Ref = ref(null)
const chart3Ref = ref(null)

let chart1 = null
let chart2 = null
let chart3 = null

const codeLines = computed(() => {
  return [
    '// ========================================',
    '//  红色数据可视化 - 完整实现代码',
    '//  Author: 你',
    '//  Date: 2026-05-26',
    '// ========================================',
    '',
    'import * as echarts from \'echarts\'',
    'import { ref, onMounted, onUnmounted } from \'vue\'',
    '',
    '// 数据配置',
    'const gdpData = [1205.1, 1318.05, 1696.27, 2043.70, 2280.24]',
    'const growthData = [6.2, 7.8, 8.5, 9.1, 7.6]',
    'const years = [\'2015\', \'2017\', \'2019\', \'2021\', \'2023\']',
    '',
    '// 图表初始化',
    'const initChart = () => {',
    '  const chartDom = document.getElementById(\'mainChart\')',
    '  const myChart = echarts.init(chartDom, \'dark\')',
    '',
    '  // 主配置项',
    '  const option = {',
    '    title: {',
    '      text: \'延安革命老区 GDP\',',
    '      left: \'center\',',
    '      textStyle: { fontSize: 18, fontWeight: 600 }',
    '    },',
    '    tooltip: {',
    '      trigger: \'axis\',',
    '      axisPointer: { type: \'cross\' },',
    '      backgroundColor: \'rgba(255,255,255,0.95)\'',
    '    },',
    '    grid: {',
    '      left: \'3%\', right: \'4%\', bottom: \'3%\', top: 60,',
    '      containLabel: true',
    '    },',
    '    xAxis: { type: \'category\', data: years },',
    '    yAxis: [',
    '      { type: \'value\', name: \'GDP(亿元)\' },',
    '      { type: \'value\', name: \'增速(%)\', position: \'right\' }',
    '    ],',
    '    series: [',
    '      {',
    '        name: \'GDP\', type: \'bar\',',
    '        data: gdpData,',
    '        itemStyle: {',
    '          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [',
    '            { offset: 0, color: \'#c93737\' },',
    '            { offset: 1, color: \'#a82c2c\' }',
    '          ]),',
    '          borderRadius: [8, 8, 0, 0],',
    '          shadowColor: \'rgba(201, 55, 55, 0.3)\',',
    '          shadowBlur: 10',
    '        },',
    '        label: { show: true, position: \'top\' }',
    '      },',
    '      {',
    '        name: \'增速\', type: \'line\',',
    '        yAxisIndex: 1, data: growthData,',
    '        smooth: true, lineStyle: { width: 4 }',
    '      }',
    '    ]',
    '  }',
    '',
    '  myChart.setOption(option)',
    '',
    '  // 响应式适配',
    '  let timer = null',
    '  window.addEventListener(\'resize\', () => {',
    '    clearTimeout(timer)',
    '    timer = setTimeout(() => myChart.resize(), 300)',
    '  })',
    '',
    '  return myChart',
    '}',
    '',
    '// 数据加载',
    'const loadData = async () => {',
    '  const res = await fetch(\'/api/chart\')',
    '  const data = await res.json()',
    '  return data',
    '}',
    '',
    '// 生命周期',
    'onMounted(() => {',
    '  loadData().then(() => initChart())',
    '})',
    '',
    'onUnmounted(() => {',
    '  // 清理逻辑',
    '})'
  ]
})

const getLineType = (line) => {
  if (line.startsWith('//')) return 'comment'
  if (['import', 'const', 'let', 'return', 'if', 'else', 'for', 'while'].some(key => line.includes(key))) return 'keyword'
  if (['{', '}', '[', ']', '(', ')'].some(ch => line.includes(ch))) return 'bracket'
  if (line.includes('\'') || line.includes('"')) return 'string'
  return 'normal'
}

const initCharts = () => {
  if (chart1Ref.value) {
    chart1 = echarts.init(chart1Ref.value)
    chart1.setOption({
      title: {
        text: '',
        left: 'center'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: { type: 'cross' },
        backgroundColor: 'rgba(255, 255, 255, 0.98)',
        borderColor: '#c93737',
        borderWidth: 1,
        textStyle: { color: '#333' },
        extraCssText: 'box-shadow: 0 4px 20px rgba(201, 55, 55, 0.2);'
      },
      legend: { 
        data: ['GDP(亿元)', '年度增速(%)'], 
        top: 10,
        textStyle: { fontSize: 13 }
      },
      grid: { left: '3%', right: '4%', bottom: '3%', top: 55, containLabel: true },
      xAxis: { 
        type: 'category', 
        data: ['2015', '2017', '2019', '2021', '2023'],
        axisLine: { lineStyle: { color: '#ddd' } },
        axisLabel: { fontSize: 13 }
      },
      yAxis: [
        { 
          type: 'value', 
          name: 'GDP(亿元)',
          axisLine: { lineStyle: { color: '#ddd' } },
          splitLine: { lineStyle: { color: '#f0f0f0', type: 'dashed' } }
        },
        { 
          type: 'value', 
          name: '增速(%)', 
          position: 'right',
          axisLine: { lineStyle: { color: '#ddd' } },
          splitLine: { show: false }
        }
      ],
      series: [
        {
          name: 'GDP(亿元)',
          type: 'bar',
          data: [1205.1, 1318.05, 1696.27, 2043.70, 2280.24],
          barWidth: '40%',
          itemStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              { offset: 0, color: '#ff6b6b' },
              { offset: 0.5, color: '#c93737' },
              { offset: 1, color: '#8b0000' }
            ]),
            borderRadius: [8, 8, 0, 0],
            shadowColor: 'rgba(201, 55, 55, 0.4)',
            shadowBlur: 15,
            shadowOffsetY: 5
          },
          emphasis: {
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: '#ff8888' },
                { offset: 1, color: '#c93737' }
              ])
            }
          },
          label: {
            show: true,
            position: 'top',
            color: '#c93737',
            fontWeight: 'bold',
            fontSize: 13
          }
        },
        {
          name: '年度增速(%)',
          type: 'line',
          yAxisIndex: 1,
          data: [6.2, 7.8, 8.5, 9.1, 7.6],
          smooth: true,
          lineStyle: { 
            color: '#f59e0b', 
            width: 4,
            shadowColor: 'rgba(245, 158, 11, 0.3)',
            shadowBlur: 10
          },
          itemStyle: { 
            color: '#f59e0b',
            borderColor: '#fff',
            borderWidth: 3
          },
          symbol: 'circle',
          symbolSize: 12,
          areaStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              { offset: 0, color: 'rgba(245, 158, 11, 0.3)' },
              { offset: 1, color: 'rgba(245, 158, 11, 0.05)' }
            ])
          }
        }
      ]
    })
  }

  if (chart2Ref.value) {
    chart2 = echarts.init(chart2Ref.value)
    chart2.setOption({
      title: { text: '', left: 'center' },
      tooltip: {
        trigger: 'item',
        backgroundColor: 'rgba(255, 255, 255, 0.98)',
        borderColor: '#c93737',
        borderWidth: 1
      },
      legend: { 
        top: 10, 
        textStyle: { fontSize: 13 }
      },
      grid: { left: '3%', right: '30%', bottom: '3%', top: 55, containLabel: true },
      xAxis: { 
        type: 'category', 
        data: ['2015', '2017', '2019', '2021', '2023'],
        axisLine: { lineStyle: { color: '#ddd' } }
      },
      yAxis: { 
        type: 'value', 
        name: '党员数量(万名)',
        axisLine: { lineStyle: { color: '#ddd' } },
        splitLine: { lineStyle: { color: '#f0f0f0', type: 'dashed' } }
      },
      series: [
        {
          name: '党员总数',
          type: 'line',
          smooth: true,
          data: [8875.8, 9059.4, 9191.4, 9671.2, 9918.5],
          lineStyle: { 
            color: '#c93737', 
            width: 4,
            shadowColor: 'rgba(201, 55, 55, 0.4)',
            shadowBlur: 10
          },
          itemStyle: { 
            color: '#c93737',
            borderColor: '#fff',
            borderWidth: 3
          },
          symbol: 'circle',
          symbolSize: 14,
          areaStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              { offset: 0, color: 'rgba(201, 55, 55, 0.4)' },
              { offset: 1, color: 'rgba(201, 55, 55, 0.05)' }
            ])
          },
          label: {
            show: true,
            position: 'top',
            color: '#c93737',
            fontWeight: 'bold',
            fontSize: 12
          }
        },
        {
          name: '城乡结构',
          type: 'pie',
          radius: ['35%', '55%'],
          center: ['80%', '50%'],
          data: [
            { 
              name: '城镇党员', 
              value: 7240, 
              itemStyle: { 
                color: new echarts.graphic.LinearGradient(0, 0, 1, 1, [
                  { offset: 0, color: '#c93737' },
                  { offset: 1, color: '#a82c2c' }
                ]),
                borderRadius: 4
              }
            },
            { 
              name: '农村党员', 
              value: 2678.5, 
              itemStyle: { 
                color: new echarts.graphic.LinearGradient(0, 0, 1, 1, [
                  { offset: 0, color: '#22c55e' },
                  { offset: 1, color: '#16a34a' }
                ]),
                borderRadius: 4
              }
            }
          ],
          label: { 
            formatter: '{b}\n{c}万\n({d}%)',
            lineHeight: 20
          },
          labelLine: { 
            length: 20,
            length2: 25,
            smooth: true
          },
          emphasis: {
            itemStyle: {
              shadowBlur: 20,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.3)'
            },
            scale: true,
            scaleSize: 10
          }
        }
      ]
    })
  }

  if (chart3Ref.value) {
    chart3 = echarts.init(chart3Ref.value)
    chart3.setOption({
      title: { text: '', left: 'center' },
      tooltip: {
        trigger: 'axis',
        axisPointer: { type: 'shadow' },
        backgroundColor: 'rgba(255, 255, 255, 0.98)',
        borderColor: '#c93737',
        borderWidth: 1
      },
      legend: { 
        data: ['剩余贫困人口(万人)', '年度脱贫人数(万人)'], 
        top: 10,
        textStyle: { fontSize: 13 }
      },
      grid: { left: '3%', right: '4%', bottom: '3%', top: 55, containLabel: true },
      xAxis: { 
        type: 'category', 
        data: ['2015', '2017', '2019', '2020中期', '2020年末'],
        axisLine: { lineStyle: { color: '#ddd' } },
        axisLabel: { fontSize: 13 }
      },
      yAxis: { 
        type: 'value', 
        name: '人数(万人)',
        axisLine: { lineStyle: { color: '#ddd' } },
        splitLine: { lineStyle: { color: '#f0f0f0', type: 'dashed' } }
      },
      series: [
        {
          name: '剩余贫困人口(万人)',
          type: 'bar',
          data: [5575, 3046, 551, 551, 0],
          barWidth: '35%',
          itemStyle: {
            color: function(params) {
              const colors = ['#ef4444', '#f97316', '#eab308', '#22c55e', '#16a34a']
              return colors[params.dataIndex]
            },
            borderRadius: [6, 6, 0, 0],
            shadowColor: 'rgba(0, 0, 0, 0.15)',
            shadowBlur: 8,
            shadowOffsetY: 3
          },
          emphasis: {
            itemStyle: {
              brightness: 1.2
            }
          },
          label: {
            show: true,
            position: 'top',
            color: '#333',
            fontWeight: 'bold',
            fontSize: 13
          }
        },
        {
          name: '年度脱贫人数(万人)',
          type: 'line',
          step: 'end',
          data: [1442, 1289, 1109, 0, 551],
          lineStyle: { 
            color: '#2563eb', 
            width: 4,
            shadowColor: 'rgba(37, 99, 235, 0.3)',
            shadowBlur: 10
          },
          itemStyle: { 
            color: '#2563eb',
            borderColor: '#fff',
            borderWidth: 3
          },
          symbol: 'diamond',
          symbolSize: 12,
          areaStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              { offset: 0, color: 'rgba(37, 99, 235, 0.25)' },
              { offset: 1, color: 'rgba(37, 99, 235, 0.02)' }
            ])
          }
        }
      ]
    })
  }
}

const restartLearning = () => {
  emit('navigate', 1)
}

const handleResize = () => {
  chart1?.resize()
  chart2?.resize()
  chart3?.resize()
}

onMounted(() => {
  initCharts()
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  chart1?.dispose()
  chart2?.dispose()
  chart3?.dispose()
})
</script>

<style scoped>
.header-content {
  animation: slideDown 0.5s ease;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.code-review-card {
  background: #1e1e1e;
  border-radius: 12px;
  padding: 24px;
  margin-bottom: 32px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
}

.review-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.review-title {
  font-size: 18px;
  font-weight: 600;
  color: white;
}

.review-status {
  font-size: 14px;
  font-weight: 500;
  color: #50fa7b;
  background: rgba(80, 250, 123, 0.1);
  padding: 6px 16px;
  border-radius: 20px;
}

.review-code {
  background: #0d0d0d;
  border-radius: 8px;
  padding: 20px;
  max-height: 380px;
  overflow-y: auto;
  border: 1px solid #333;
}

.code-line {
  display: flex;
  gap: 12px;
  font-family: 'Consolas', 'Monaco', 'Courier New', monospace;
  font-size: 13px;
  line-height: 1.9;
  color: #f8f8f2;
}

.line-number {
  color: #6272a4;
  user-select: none;
  min-width: 36px;
  text-align: right;
  opacity: 0.6;
}

.line-text.comment {
  color: #6272a4;
  font-style: italic;
}

.line-text.keyword {
  color: #ff79c6;
  font-weight: 600;
}

.line-text.string {
  color: #f1fa8c;
}

.line-text.bracket {
  color: #ff79c6;
}

.line-text.normal {
  color: #f8f8f2;
}

.charts-section {
  display: flex;
  flex-direction: column;
  gap: 28px;
  margin-bottom: 32px;
}

.chart-card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  animation: fadeInUp 0.6s ease forwards;
  opacity: 0;
  border: 1px solid rgba(201, 55, 55, 0.1);
}

.chart-card:nth-child(1) { animation-delay: 0.1s; }
.chart-card:nth-child(2) { animation-delay: 0.25s; }
.chart-card:nth-child(3) { animation-delay: 0.4s; }

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
  from {
    transform: translateY(30px);
  }
}

.chart-label {
  padding: 20px 24px;
  border-bottom: 1px solid #f0f0f0;
  display: flex;
  align-items: center;
  gap: 16px;
  background: linear-gradient(135deg, rgba(201, 55, 55, 0.03), rgba(168, 44, 44, 0.02));
}

.chart-number {
  font-size: 13px;
  font-weight: 700;
  color: white;
  background: linear-gradient(135deg, #c93737, #a82c2c);
  padding: 6px 14px;
  border-radius: 6px;
  letter-spacing: 1px;
}

.chart-label h3 {
  font-size: 17px;
  font-weight: 700;
  color: #1d1d1f;
  margin: 0;
  letter-spacing: 0.3px;
}

.chart-item {
  width: 100%;
  height: 380px;
}

.summary-card {
  text-align: center;
  padding: 48px 32px;
  background: linear-gradient(135deg, rgba(201, 55, 55, 0.06), rgba(168, 44, 44, 0.04));
  border-radius: 16px;
  border: 2px dashed rgba(201, 55, 55, 0.25);
}

.summary-icon {
  font-size: 72px;
  margin-bottom: 20px;
}

.summary-card h2 {
  font-size: 30px;
  font-weight: 700;
  color: #c93737;
  margin: 0 0 10px;
  letter-spacing: 1px;
}

.summary-card p {
  font-size: 17px;
  color: #666;
  margin: 0 0 32px;
}

.summary-stats {
  display: flex;
  justify-content: center;
  gap: 56px;
  margin-bottom: 32px;
}

.stat {
  text-align: center;
}

.stat-value {
  font-size: 42px;
  font-weight: 800;
  color: #c93737;
  line-height: 1;
  margin-bottom: 8px;
}

.stat-label {
  font-size: 15px;
  color: #666;
  font-weight: 500;
}

@media screen and (max-width: 768px) {
  .code-review-card {
    padding: var(--spacing-lg);
    margin-bottom: var(--spacing-xl);
  }
  
  .review-title {
    font-size: 16px;
  }
  
  .review-status {
    font-size: 12px;
    padding: 4px 12px;
  }
  
  .review-code {
    padding: 16px;
    max-height: 300px;
  }
  
  .code-line {
    font-size: 12px;
    line-height: 1.7;
  }
  
  .line-number {
    min-width: 32px;
  }
  
  .charts-section {
    gap: var(--spacing-lg);
    margin-bottom: var(--spacing-xl);
  }
  
  .chart-card {
    border-radius: var(--radius-lg);
  }
  
  .chart-label {
    padding: var(--spacing-md) var(--spacing-lg);
    gap: var(--spacing-sm);
  }
  
  .chart-number {
    font-size: 11px;
    padding: 4px 10px;
  }
  
  .chart-label h3 {
    font-size: 15px;
  }
  
  .chart-item {
    height: 280px;
  }
  
  .summary-card {
    padding: var(--spacing-xl);
    border-radius: var(--radius-lg);
  }
  
  .summary-icon {
    font-size: 56px;
    margin-bottom: var(--spacing-md);
  }
  
  .summary-card h2 {
    font-size: 24px;
  }
  
  .summary-card p {
    font-size: 15px;
    margin-bottom: var(--spacing-xl);
  }
  
  .summary-stats {
    gap: var(--spacing-xl);
    margin-bottom: var(--spacing-xl);
  }
  
  .stat-value {
    font-size: 32px;
  }
  
  .stat-label {
    font-size: 13px;
  }
}

@media screen and (max-width: 480px) {
  .code-review-card {
    padding: var(--spacing-md);
  }
  
  .review-code {
    padding: 12px;
    max-height: 250px;
  }
  
  .code-line {
    font-size: 11px;
    line-height: 1.5;
  }
  
  .line-number {
    min-width: 28px;
  }
  
  .chart-label {
    padding: var(--spacing-sm) var(--spacing-md);
  }
  
  .chart-label h3 {
    font-size: 14px;
  }
  
  .chart-item {
    height: 240px;
  }
  
  .summary-card {
    padding: var(--spacing-lg);
  }
  
  .summary-icon {
    font-size: 48px;
  }
  
  .summary-card h2 {
    font-size: 22px;
  }
  
  .summary-card p {
    font-size: 14px;
  }
  
  .summary-stats {
    gap: var(--spacing-lg);
  }
  
  .stat-value {
    font-size: 28px;
  }
  
  .stat-label {
    font-size: 12px;
  }
}

@media screen and (max-width: 360px) {
  .chart-item {
    height: 220px;
  }
  
  .summary-icon {
    font-size: 40px;
  }
  
  .summary-card h2 {
    font-size: 20px;
  }
  
  .stat-value {
    font-size: 24px;
  }
}
</style>
