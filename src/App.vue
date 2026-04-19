<template>
  <div class="world" :class="{ 'glitch-mode': isGlitching, 'hyper-mode': isHyper }">
    <div class="bg-noise"></div>

    <header class="top-bar">
      <div class="title-group">
        <h1 class="main-title" :data-text="pageTitle">{{ pageTitle }}</h1>
        <p class="subtitle">底层逻辑篡改者 · 万象界</p>
      </div>
      <div class="countdown-group">
        <div class="clock">{{ timer }}</div>
        <p class="clock-label">2029 USTC 22408 DOOMSDAY</p>
      </div>
    </header>

    <main class="distorted-containment">
      <div class="fragmented-bottle">
        <svg width="0" height="0">
          <defs>
            <clipPath id="fractureShape" clipPathUnits="objectBoundingBox">
              <path d="M 0.15 0.02 L 0.85 0.02 L 0.98 0.45 L 0.9 0.98 L 0.1 0.98 L 0.02 0.45 Z"></path>
            </clipPath>
          </defs>
        </svg>
        <div class="ink-content">
          <canvas id="ink"></canvas>
        </div>
        <div class="bottle-frame"></div>
      </div>

      <aside class="goal-panel">
        <div class="panel-header">GOAL_ASSIMILATION_LOG</div>
        <div class="goal-display">
          <span class="goal-prefix">VOCAB_</span>
          <div class="percentage">{{ wordProgress }}<span class="percent-sign">%</span></div>
        </div>
        <div class="panel-status">STATUS: SIPHONING :: IDLE</div>
        <p class="panel-flavor">因果漂移检测：正常 / 逻辑污染：低</p>
        <div class="sub-targets">
          <span class="tag">C-LANG</span>
          <span class="tag">DATA-STRUCT</span>
          <span class="tag">D diligence</span>
        </div>
      </aside>
    </main>

    <div class="altar">
      <input v-model="insight" @keyup.enter="handleCommand" placeholder="输入今日悟道所得或真名指令..." />
      <div class="altar-stats">
        <span>DOJO: 3-ON-2-OFF</span>
        <span>CAFFEINE: {{ currentCaffeine }}MG</span>
        <span>KARMA: {{ karmaValue }}</span>
      </div>
    </div>
    
    <div class="pollution-overlay" v-if="isGlitching"></div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// --- 状态与核心变量 ---
const timer = ref('载入中...')
const wordProgress = ref(45) 
const insight = ref('')
const pageTitle = ref("Vzuor Khaa-Sh'an")
const currentCaffeine = ref(200)
const karmaValue = ref(0)

// --- 特效控制开关 ---
const isGlitching = ref(false)
const isHyper = ref(false)

// --- 核心逻辑：精准计算 2029 届（2028年底初试）命数 ---
const updateTimer = () => {
  // 预计 2029 届初试在 2028 年 12 月 23 日左右
  const target = new Date('2028-12-23T08:30:00').getTime() 
  const now = new Date().getTime()
  const diff = target - now

  if (diff <= 0) {
    timer.value = "因果已至"
    return
  }

  const d = Math.floor(diff / (1000 * 60 * 60 * 24))
  const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
  const s = Math.floor((diff % (1000 * 60)) / 1000)

  // 格式：天数D | 时:分:秒
  timer.value = `${d}D | ${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`
}

// --- 交互彩蛋处理器 (指令解析) ---
const handleCommand = () => {
  const cmd = insight.value.trim().toLowerCase()
  if (!cmd) return

  switch (cmd) {
    case 'vzuor':
      triggerTrueNameEffect()
      break
    case '200mg':
    case 'caffeine':
      triggerHyperModeEffect()
      break
    default:
      // 普通输入增加业障值，水位轻微上涨
      karmaValue.value += Math.floor(Math.random() * 5) + 1
      break
  }
  insight.value = ''
}

// 特效：真名降临
const triggerTrueNameEffect = () => {
  isGlitching.value = true
  setTimeout(() => isGlitching.value = false, 2500)
}

// 特效：狂化模式 (200mg)
const triggerHyperModeEffect = () => {
  isHyper.value = true
  currentCaffeine.value = 400
  setTimeout(() => { isHyper.value = false; currentCaffeine.value = 200 }, 5000)
}

// --- Canvas 墨水与深渊凝视逻辑 ---
onMounted(() => {
  const cvs = document.getElementById('ink')
  const ctx = cvs.getContext('2d')
  cvs.width = 240; cvs.height = 300
  let t = 0
  
  // 随机出现的深渊之眼
  let eyeX = 0, eyeY = 0, eyeLife = 0

  function animate() {
    ctx.clearRect(0, 0, 240, 300)
    
    // 如果处于狂化模式，墨水变红
    ctx.fillStyle = isHyper.value ? '#8B0000' : '#1A1A1A'
    
    // 计算基于进度的水位高度
    const baseHeight = 300 * (1 - wordProgress.value / 100)
    const level = baseHeight - 10 
    
    // 绘制流体波浪
    ctx.beginPath()
    ctx.moveTo(0, level)
    for (let x = 0; x <= 240; x++) {
      // 增加波浪的混沌感
      const speed = isHyper.value ? 0.12 : 0.06
      const amplitude = isHyper.value ? 15 : 8
      const y = level + Math.sin(x * speed + t) * amplitude + Math.cos(x * 0.01 + t * 0.5) * 4
      ctx.lineTo(x, y)
    }
    ctx.lineTo(240, 300); ctx.lineTo(0, 300)
    ctx.fill()
    
    // 深渊之眼逻辑 (低概率在墨水中出现)
    if (eyeLife <= 0 && Math.random() < 0.005) {
      eyeX = 40 + Math.random() * 160
      eyeY = level + 40 + Math.random() * 120
      eyeLife = 60 // 存活帧数
    }

    if (eyeLife > 0) {
      // 绘制一只红色的眼睛
      ctx.fillStyle = 'rgba(255, 0, 0, ' + (eyeLife / 60) + ')'
      ctx.beginPath()
      ctx.ellipse(eyeX, eyeY, 8, 3, 0, 0, Math.PI * 2) // 眼睛轮廓
      ctx.fill()
      ctx.fillStyle = '#000'
      ctx.beginPath()
      ctx.arc(eyeX, eyeY, 2, 0, Math.PI * 2) // 瞳孔
      ctx.fill()
      eyeLife--
    }

    t += isHyper.value ? 0.15 : 0.07 
    requestAnimationFrame(animate)
  }
  animate()

  // 启动倒计时
  updateTimer()
  setInterval(updateTimer, 1000)
})
</script>

<style>
/* 引用宿命字体 (Cinzel) */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&display=swap');

/* --- 全局：羊皮纸底色与噪音颗粒 --- */
:root { background: #F7F5F0; }
body { margin: 0; padding: 0; background: #F7F5F0; font-family: "Cinzel", serif; color: #2C2C2C; overflow-x: hidden; -webkit-font-smoothing: antialiased; }

/* 静默噪音颗粒图层 */
.bg-noise { position: fixed; inset: 0; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="noiseFilter"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="3" stitchTiles="stitch"/></filter><rect width="100" height="100" filter="url(%23noiseFilter)"/></svg>'); opacity: 0.02; pointer-events: none; z-index: 999; }

.world { min-height: 100vh; display: flex; flex-direction: column; align-items: center; padding: 40px; position: relative; transition: all 0.5s ease; }

/* 狂化模式 (200mg触发) */
.hyper-mode { filter: saturate(1.5) contrast(1.1); }
.hyper-mode .percentage { color: #8B0000; text-shadow: 2px 2px 0 rgba(139,0,0,0.2); }
.hyper-mode .main-title { animation: shake 0.5s infinite; }

/* 故障模式 (真名触发) */
.glitch-mode .main-title::before, .glitch-mode .main-title::after { content: attr(data-text); position: absolute; top: 0; left: 0; width: 100%; height: 100%; opacity: 0.8; }
.glitch-mode .main-title::before { left: 2px; text-shadow: -1px 0 red; animation: glitch-anim 0.3s infinite linear alternate-reverse; }
.glitch-mode .main-title::after { left: -2px; text-shadow: -1px 0 blue; animation: glitch-anim 0.2s infinite linear alternate-reverse; }

.pollution-overlay { position: fixed; inset: 0; border: 15px solid rgba(139, 0, 0, 0.4); pointer-events: none; z-index: 1000; animation: pulse-border 1s infinite; }

/* 顶部布局 */
.top-bar { width: 100%; max-width: 900px; display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 80px; position: relative; z-index: 10; }
.main-title { font-size: 2rem; margin: 0; letter-spacing: 2px; text-transform: uppercase; font-weight: bold; position: relative; }
.subtitle { font-size: 0.7rem; opacity: 0.5; margin: 5px 0; letter-spacing: 4px; text-transform: uppercase; font-family: sans-serif; }
.countdown-group { text-align: right; }
.clock { font-size: 2.5rem; font-weight: bold; font-family: 'Courier New', monospace; letter-spacing: -2px; }
.clock-label { font-size: 0.6rem; opacity: 0.4; margin: 0; text-align: right; letter-spacing: 1px; }

/* 中部核心：裂变畸变容器 */
.distorted-containment { display: flex; align-items: center; gap: 50px; margin-bottom: 60px; z-index: 10; position: relative; }

/* 裂变瓶体 (核心视觉修改点) */
.fragmented-bottle { position: relative; width: 220px; height: 320px; }
.ink-content { width: 100%; height: 100%; background: #fff; box-shadow: 0 20px 50px rgba(0,0,0,0.1); overflow: hidden; /* 应用碎片裁剪 */ -webkit-clip-path: url(#fractureShape); clip-path: url(#fractureShape); /* 应用不稳定的伪3D畸变滤镜 */ filter: contrast(1.1) brightness(1.02) blur(0.2px) hue-rotate(-5deg); transition: filter 0.3s; }
.fragmented-bottle:hover .ink-content { filter: contrast(1.2) brightness(1.05) blur(0.5px) hue-rotate(5deg); }
#ink { position: absolute; bottom: 0; left: 0; width: 100%; height: 100%; }
.bottle-frame { position: absolute; inset: -5px; border: 2px dashed #2C2C2C; -webkit-clip-path: url(#fractureShape); clip-path: url(#fractureShape); opacity: 0.2; pointer-events: none; }

/* 因果面板 (原本在瓶子里，移至此) */
.goal-panel { border: 1px solid rgba(44, 44, 44, 0.1); background: rgba(255,255,255,0.3); padding: 25px 30px; border-radius: 2px; text-align: left; position: relative; width: 300px; box-sizing: border-box; font-family: sans-serif; }
.goal-panel::before { content: ''; position: absolute; top: -10px; left: 10px; width: 20px; height: 2px; background: rgba(0,0,0,0.5); }
.goal-panel::after { content: ''; position: absolute; bottom: -1px; right: 10px; width: 2px; height: 10px; background: rgba(0,0,0,0.5); }

.panel-header { font-size: 10px; letter-spacing: 3px; opacity: 0.3; margin-bottom: 20px; text-transform: uppercase; font-weight: bold; }
.goal-display { position: relative; margin-bottom: 15px; }
.goal-prefix { font-size: 11px; letter-spacing: 1px; opacity: 0.2; position: absolute; top: 0; left: 0; }
.percentage { font-size: 5rem; font-weight: 900; line-height: 1; letter-spacing: -6px; margin-left: -5px; color: #1A1A1A; font-family: "Cinzel", serif; }
.percent-sign { font-size: 1.5rem; letter-spacing: 0; vertical-align: super; opacity: 0.3; margin-left: 5px; }

.panel-status { font-size: 11px; font-family: monospace; letter-spacing: 1px; margin-bottom: 8px; font-weight: bold; color: rgba(0,0,0,0.8); }
.panel-flavor { font-size: 9px; opacity: 0.5; margin: 0; font-family: sans-serif; line-height: 1.4; color: #8B0000; }

.sub-targets { display: flex; gap: 8px; margin-top: 15px; }
.tag { font-size: 8px; padding: 3px 6px; border: 1px solid rgba(0,0,0,0.2); border-radius: 2px; opacity: 0.6; letter-spacing: 1px; }

/* 底部修齐台 */
.altar { margin-top: auto; width: 450px; text-align: center; border-bottom: 1px solid rgba(0,0,0,0.1); margin-bottom: 40px; z-index: 10; position: relative; }
input { width: 100%; background: none; border: none; outline: none; padding: 20px; text-align: center; font-style: italic; font-size: 1.2rem; color: #2C2C2C; font-family: sans-serif; letter-spacing: 1px; }
input::placeholder { color: rgba(0,0,0,0.2); font-style: normal; }
.altar-stats { display: flex; justify-content: space-between; padding: 12px 10px; font-size: 0.6rem; opacity: 0.5; letter-spacing: 2px; font-family: monospace; text-transform: uppercase; }

/* 动画定义 */
@keyframes shake { 0% { transform: translate(1px, 1px) rotate(0deg); } 25% { transform: translate(-1px, -2px) rotate(-1deg); } 50% { transform: translate(-3px, 0px) rotate(1deg); } 75% { transform: translate(3px, 2px) rotate(0deg); } 100% { transform: translate(1px, -1px) rotate(0deg); } }
@keyframes glitch-anim { 0% { clip: rect(10px, 9999px, 30px, 0); } 20% { clip: rect(50px, 9999px, 60px, 0); } 40% { clip: rect(20px, 9999px, 40px, 0); } 60% { clip: rect(70px, 9999px, 80px, 0); } 80% { clip: rect(5px, 9999px, 15px, 0); } 100% { clip: rect(35px, 9999px, 50px, 0); } }
@keyframes pulse-border { 0% { border-color: rgba(139, 0, 0, 0.2); } 50% { border-color: rgba(139, 0, 0, 0.6); } 100% { border-color: rgba(139, 0, 0, 0.2); } }
</style>