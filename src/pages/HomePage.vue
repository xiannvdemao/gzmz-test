<template>
  <div class="page-container home-page">
    <div class="home-header">
      <div class="home-hero">
        <div class="hero-icon">
          <svg viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="32" cy="32" r="30" fill="url(#grad1)"/>
            <path d="M32 14v36M14 32h36" stroke="white" stroke-width="4" stroke-linecap="round"/>
            <circle cx="32" cy="32" r="8" fill="white"/>
            <defs>
              <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="100%">
                <stop offset="0%" stop-color="#c93737"/>
                <stop offset="100%" stop-color="#a82c2c"/>
              </linearGradient>
            </defs>
          </svg>
        </div>
        <h1 class="home-title">红色数据可视化分析实践</h1>
        <p class="home-subtitle">践行求真务实学风 · 规范编写代码 · 传承红色基因</p>
      </div>
      <div class="home-stats">
        <div class="stat-item">
          <div class="stat-value">6</div>
          <div class="stat-label">核心知识点</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">5</div>
          <div class="stat-label">编程练习</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">3</div>
          <div class="stat-label">可视化图表</div>
        </div>
      </div>
      <button class="home-start-btn" @click="$emit('navigate', 2)">
                <span class="btn-text">开始学习之旅</span>
                <span class="btn-sub">掌握数据可视化知识</span>
                <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M5 12h14M12 5l7 7-7 7"/>
                </svg>
            </button>
    </div>
    <div class="home-features">
      <div class="feature-card">
        <div class="feature-icon">📊</div>
        <h3>数据可视化</h3>
        <p>基于ECharts 5.x实现专业级数据可视化图表</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon">💻</div>
        <h3>工程化开发</h3>
        <p>遵循企业级开发规范，代码结构清晰可维护</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon">🎯</div>
        <h3>实践导向</h3>
        <p>理论与实践相结合，边学边练边应用</p>
      </div>
    </div>
  </div>
</template>

<script setup>
defineEmits(['navigate'])
</script>

<style scoped>
.home-page {
  padding: 0;
}

.home-header {
  padding: var(--spacing-xxl) var(--spacing-lg);
  text-align: center;
}

.home-hero {
  margin-bottom: var(--spacing-xxl);
}

.hero-icon {
  width: 100px;
  height: 100px;
  margin: 0 auto var(--spacing-xl);
}

.home-title {
  font-size: 32px;
  font-weight: 600;
  color: var(--color-primary);
  letter-spacing: -0.5px;
  margin-bottom: var(--spacing-md);
}

.home-subtitle {
  font-size: 16px;
  color: var(--color-text-secondary);
  line-height: 1.6;
}

.home-stats {
  display: flex;
  justify-content: center;
  gap: var(--spacing-xl);
  margin-bottom: var(--spacing-xxl);
}

.stat-item {
  text-align: center;
}

.stat-value {
  font-size: 36px;
  font-weight: 700;
  color: var(--color-primary);
  margin-bottom: var(--spacing-xs);
}

.stat-label {
  font-size: 14px;
  color: var(--color-text-secondary);
}

.home-start-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 20px 40px;
  font-size: 20px;
  font-weight: 700;
  color: white;
  background: linear-gradient(90deg, var(--color-primary), var(--color-primary-dark));
  border: none;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 6px 24px rgba(201, 55, 55, 0.35);
  position: relative;
  overflow: hidden;
  width: 100%;
  max-width: 360px;
  margin: 0 auto;
}

.home-start-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.15), transparent);
  transition: left 0.4s ease;
}

.home-start-btn:hover::before {
  left: 100%;
}

.home-start-btn:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 32px rgba(201, 55, 55, 0.45);
}

.home-start-btn:active {
  transform: translateY(-2px);
}

.home-start-btn .btn-text {
  display: flex;
  align-items: center;
  gap: 12px;
}

.home-start-btn .btn-sub {
  font-size: 14px;
  font-weight: 500;
  opacity: 0.95;
}

.arrow-icon {
  width: 22px;
  height: 22px;
}

.home-features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--spacing-lg);
  padding: var(--spacing-xl);
  background: var(--color-surface-parchment);
}

.feature-card {
  background: white;
  padding: var(--spacing-xl);
  border-radius: var(--radius-lg);
  text-align: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  transition: all 0.25s ease;
}

.feature-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
}

.feature-icon {
  font-size: 40px;
  margin-bottom: var(--spacing-md);
}

.feature-card h3 {
  font-size: 18px;
  font-weight: 600;
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-sm);
}

.feature-card p {
  font-size: 14px;
  color: var(--color-text-secondary);
  line-height: 1.5;
}

@media screen and (max-width: 768px) {
  .home-header {
    padding: var(--spacing-xl) var(--spacing-md);
  }
  
  .home-hero {
    margin-bottom: var(--spacing-xl);
  }
  
  .hero-icon {
    width: 80px;
    height: 80px;
    margin: 0 auto var(--spacing-lg);
  }
  
  .home-title {
    font-size: 28px;
  }
  
  .home-subtitle {
    font-size: 14px;
  }
  
  .home-stats {
    gap: var(--spacing-lg);
    margin-bottom: var(--spacing-xl);
  }
  
  .stat-value {
    font-size: 28px;
  }
  
  .stat-label {
    font-size: 13px;
  }
  
  .home-start-btn {
    max-width: 100%;
    padding: 18px 24px;
    font-size: 18px;
  }
  
  .home-features {
    grid-template-columns: 1fr;
    gap: var(--spacing-md);
    padding: var(--spacing-lg);
  }
  
  .feature-card {
    padding: var(--spacing-lg);
  }
  
  .feature-icon {
    font-size: 36px;
  }
  
  .feature-card h3 {
    font-size: 16px;
  }
  
  .feature-card p {
    font-size: 13px;
  }
}

@media screen and (max-width: 480px) {
  .home-header {
    padding: var(--spacing-lg) var(--spacing-md);
  }
  
  .hero-icon {
    width: 70px;
    height: 70px;
    margin: 0 auto var(--spacing-md);
  }
  
  .home-title {
    font-size: 24px;
  }
  
  .home-subtitle {
    font-size: 13px;
  }
  
  .home-stats {
    gap: var(--spacing-md);
  }
  
  .stat-value {
    font-size: 24px;
  }
  
  .stat-label {
    font-size: 12px;
  }
  
  .feature-card {
    padding: var(--spacing-md);
  }
  
  .feature-icon {
    font-size: 32px;
  }
}

@media screen and (max-width: 360px) {
  .home-title {
    font-size: 22px;
  }
  
  .home-subtitle {
    font-size: 12px;
    line-height: 1.4;
  }
  
  .stat-value {
    font-size: 22px;
  }
  
  .feature-icon {
    font-size: 28px;
  }
}
</style>
