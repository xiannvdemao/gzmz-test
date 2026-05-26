<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$emit('navigate', 1)">←</button>
      <h1 class="page-title">数据可视化核心知识</h1>
      <p class="page-subtitle">主流前端技术 · 企业级开发规范</p>
    </div>
    <div class="divider"></div>
    <div class="page-content">
      <KnowledgeCard
        v-for="(item, index) in knowledgeItems"
        :key="index"
        :index="index"
        :title="item.title"
        :description="item.description"
        :code="item.code"
      />
      <button class="next-step-btn" @click="$emit('navigate', 3)">
        <span class="btn-text">继续下一步</span>
        <span class="btn-sub">开始代码实践</span>
        <svg class="btn-arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M5 12h14M12 5l7 7-7 7"/>
        </svg>
      </button>
    </div>
  </div>
</template>

<script setup>
import KnowledgeCard from '../components/KnowledgeCard.vue'

defineEmits(['navigate'])

const knowledgeItems = [
  {
    title: '技术栈：ECharts 5.x + 原生JS / Vue3 / React，是目前数据可视化主流方案。',
    description: [
      'ECharts 5.x 是百度开源的可视化库，兼容原生JS、Vue3、React等框架，支持千万级数据渲染，是企业级大屏、报表的首选方案。',
      '• 原生JS：适合简单页面，直接引入CDN即可使用',
      '• Vue3/React：适合复杂SPA项目，通过npm安装，组件化开发'
    ],
    code: `// 原生JS 引入方式
// npm install echarts

// Vue3 安装与使用
npm install echarts
import * as echarts from 'echarts'

// React 安装与使用
npm install echarts-for-react
import ReactECharts from 'echarts-for-react'`
  },
  {
    title: '数据获取：从权威数据源获取延安GDP等红色数据。',
    description: [
      '获取真实可靠的红色数据是可视化的基础，可从以下渠道获取：',
      '• 政府官方网站：国家统计局、延安市人民政府官网',
      '• 权威数据库：中国统计年鉴、地方统计公报',
      '• 公开API：部分地方政府提供数据开放接口',
      '• 数据格式：JSON、CSV是最常用的格式'
    ],
    code: `// 从API获取延安GDP数据示例
async function fetchYanAnGDP() {
  try {
    const response = await fetch('/api/yangan-gdp');
    const rawData = await response.json();
    return rawData;
  } catch (error) {
    console.error('数据获取失败:', error);
    return null;
  }
}

// 从公开CSV数据解析
async function loadCSVData(url) {
  const response = await fetch(url);
  const csvText = await response.text();
  return parseCSV(csvText);
}`
  },
  {
    title: '数据清洗：处理缺失值、格式转换、标准化，确保数据质量。',
    description: [
      '原始数据往往存在各种问题，需要清洗才能使用：',
      '• 缺失值处理：插值填充、删除或标记缺失数据',
      '• 格式转换：字符串转数字、日期格式统一',
      '• 异常值检测：识别并处理异常数据点',
      '• 数据验证：检查数据范围和合理性'
    ],
    code: `// 数据清洗完整流程
function cleanGDPData(rawData) {
  return rawData.map(item => {
    // 1. 处理缺失值
    const gdp = item.gdp ?? 0;
    const growth = item.growth ?? 0;
    
    // 2. 数据类型转换
    const cleanItem = {
      year: parseInt(item.year),
      gdp: parseFloat(gdp),
      growth: parseFloat(growth)
    };
    
    // 3. 数据验证
    if (cleanItem.gdp < 0 || cleanItem.gdp > 10000) {
      console.warn('异常数据:', item);
      return null;
    }
    
    return cleanItem;
  }).filter(Boolean); // 4. 过滤无效数据
}`
  },
  {
    title: '渲染流程：获取DOM → 初始化图表 → 配置option → 渲染 → 响应式适配。',
    description: [
      '标准渲染流程是所有ECharts项目的核心逻辑，确保图表正确初始化、渲染并适配窗口变化。',
      '1. 获取DOM：通过document.getElementById获取图表容器',
      '2. 初始化：echarts.init(dom)创建图表实例',
      '3. 配置option：定义图表标题、坐标轴、系列数据',
      '4. 渲染：setOption(option)将配置应用到图表',
      '5. 响应式：监听resize事件，调用resize()适配窗口'
    ],
    code: `// 完整渲染流程代码
const dom = document.getElementById('chart');
const myChart = echarts.init(dom);
const option = {
  title: { text: '示例图表' },
  xAxis: { type: 'category', data: ['A','B','C'] },
  yAxis: { type: 'value' },
  series: [{ type: 'bar', data: [10,20,30] }]
};
myChart.setOption(option);
window.addEventListener('resize', () => myChart.resize());`
  },
  {
    title: '核心API：echarts.init() / setOption() / resize()，是必须掌握的基础方法。',
    description: [
      '这三个API是ECharts的核心，所有图表操作都基于它们。',
      '• <code>echarts.init(dom, theme, opts)</code>：初始化图表实例',
      '• <code>setOption(option)</code>：设置图表配置',
      '• <code>resize()</code>：手动触发图表重绘'
    ],
    code: `// 核心API使用示例
const dom = document.getElementById('chart');
const myChart = echarts.init(dom, 'dark');
myChart.setOption({
  title: { text: '核心API示例' },
  series: [{ type: 'line', data: [5, 20, 36, 10] }]
});
window.addEventListener('resize', () => myChart.resize());`
  },
  {
    title: '工程化：函数封装、异步请求、防抖优化、错误捕获、代码模块化。',
    description: [
      '工程化是企业级开发的核心要求，提升代码可维护性、性能和稳定性。',
      '• 函数封装：将图表渲染逻辑封装为独立函数',
      '• 异步请求：使用fetch/axios从后端获取数据',
      '• 防抖优化：避免窗口resize时频繁重绘图表',
      '• 错误捕获：try-catch处理图表渲染异常',
      '• 模块化：将图表组件拆分为独立文件，便于复用'
    ],
    code: `// 工程化代码示例
async function renderChart() {
  try {
    const res = await fetch('/api/chart-data');
    const data = await res.json();
    const dom = document.getElementById('chart');
    const myChart = echarts.init(dom);
    myChart.setOption({
      xAxis: { data: data.labels },
      series: [{ type: 'bar', data: data.values }]
    });
    let timer = null;
    window.addEventListener('resize', () => {
      clearTimeout(timer);
      timer = setTimeout(() => myChart.resize(), 300);
    });
  } catch (err) {
    console.error('图表渲染失败:', err);
  }
}
renderChart();`
  }
]
</script>

<style scoped>
.next-step-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  padding: 20px 40px;
  margin-top: 36px;
  width: 100%;
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
  font-size: 18px;
  font-weight: 700;
  color: white;
  background: linear-gradient(90deg, var(--color-primary), var(--color-primary-dark));
  border: none;
  border-radius: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 6px 24px rgba(201, 55, 55, 0.35);
  position: relative;
  overflow: hidden;
}

.next-step-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.15), transparent);
  transition: left 0.4s ease;
}

.next-step-btn:hover::before {
  left: 100%;
}

.next-step-btn:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 32px rgba(201, 55, 55, 0.45);
}

.next-step-btn:active {
  transform: translateY(-2px);
  box-shadow: 0 4px 16px rgba(201, 55, 55, 0.3);
}

.next-step-btn .btn-text {
  display: flex;
  align-items: center;
  gap: 10px;
}

.next-step-btn .btn-sub {
  font-size: 13px;
  font-weight: 500;
  opacity: 0.95;
}

.next-step-btn .btn-arrow {
  width: 22px;
  height: 22px;
  transition: transform 0.3s ease;
}

.next-step-btn:hover .btn-arrow {
  transform: translateX(4px);
}

@media screen and (max-width: 768px) {
  .next-step-btn {
    padding: 18px 32px;
    font-size: 16px;
    border-radius: 14px;
    margin-top: 28px;
  }
  
  .next-step-btn .btn-sub {
    font-size: 12px;
  }
  
  .next-step-btn .btn-arrow {
    width: 20px;
    height: 20px;
  }
}

@media screen and (max-width: 480px) {
  .next-step-btn {
    padding: 16px 24px;
    font-size: 15px;
    border-radius: 12px;
    margin-top: 24px;
  }
  
  .next-step-btn .btn-sub {
    font-size: 11px;
  }
}
</style>
