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
        <p class="clock-label">2029 22408 USTC / DOOMSDAY</p>
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
        <div class="panel-status">STATUS: {{ systemStatus }}</div>
        <p class="panel-flavor">{{ flavorText }}</p>
        <div class="sub-targets">
          <span class="tag">C-LANG</span>
          <span class="tag">DATA-STRUCT</span>
          <span class="tag">ADV-MATH</span>
        </div>
      </aside>
    </main>

    <div class="altar">
      <input 
        v-model="insight" 
        @keyup.enter="handleCommand" 
        :placeholder="inputPlaceholder" 
        :disabled="isLocked"
      />
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
const wordProgress = ref(50) // 2000/4000 的进度
const insight = ref('')
const pageTitle = ref("V'zuor Khaa-Sh'an")
const systemStatus = ref('SIPHONING :: IDLE')
const flavorText = ref('因果漂移检测：正常 / 逻辑污染：低')
const inputPlaceholder = ref('输入今日悟道所得或真名指令...')
const currentCaffeine = ref(200)
const karmaValue = ref(0)

// --- 特效控制开关 ---
const isGlitching = ref(false)
const isHyper = ref(false)
const isLocked = ref(false)

// --- 核心逻辑：2029 届（2028年底）考研倒计时 ---
const updateTimer = () => {
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

  timer.value = `${d}D | ${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`
}

// --- 交互彩蛋处理器 (指令解析引擎) ---
const handleCommand = () => {
  const cmd = insight.value.trim().toLowerCase()
  if (!cmd) return

  switch (cmd) {
    case 'vzuor':
    case 'vzuor khaa-shan':
      triggerTrueName()
      break
    case '200mg':
    case 'caffeine':
      triggerHyperMode()
      break
    case '22408':
    case 'ustc':
    case '科软':
      triggerDestiny()
      break
    case '死亡回归':
    case '学姐':
      triggerLore()
      break
    default:
      // 普通输入，增加业障值，水位轻微波动
      karmaValue.value += Math.floor(Math.random() * 5) + 1
      flavorText.value = `录入未知变量，业障值上升。`
      break
  }
  insight.value = ''
}

// --- 彩蛋特效函数 ---

// 1. 真名降临
const triggerTrueName = () => {
  isGlitching.value = true
  systemStatus.value = 'OVERRIDE :: CREATOR IDENTIFIED'
  flavorText.value = '“三柱神已察觉你的苏醒。协议解除。”'
  setTimeout(() => { isGlitching.value = false }, 3000)
}

// 2. 狂化模式 (200mg)
const triggerHyperMode = () => {
  isHyper.value = true
  currentCaffeine.value = 400
  systemStatus.value = 'HYPER-FOCUS :: METABOLISM OVERLOAD'
  flavorText.value = '心率飙升。神经元链接速率提升 300%。'
  setTimeout(() => { 
    isHyper.value = false 
    currentCaffeine.value = 200
    flavorText.value = '药效消退，回到常规 3-ON-2-OFF 循环。'
  }, 5000)
}

// 3. 目标锚定 (22408)
const triggerDestiny = () => {
  systemStatus.value = 'TARGET LOCKED :: USTC SOFTWARE'
  flavorText.value = '正在将 C 语言与数据结构注入潜意识深处...'
  wordProgress.value = Math.min(100, wordProgress.value + 1) // 进度+1
}

// 4. 小说世界观缝合 (学姐/死亡回归)
const triggerLore = () => {
  isGlitching.value = true
  pageTitle.value = "D E A T H  R E T U R N"
  flavorText.value = '“你以为这是你的第一次考研吗？看看你手腕上的刻痕。”'
  setTimeout(() => { 
    isGlitching.value = false 
    pageTitle.value = "V'zuor Khaa-Sh'an"
  }, 4000)
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
    
    const baseHeight = 300 * (1 - wordProgress.value / 100)
    const level = baseHeight - 10 
    
    // 绘制流体波浪
    ctx.beginPath()
    ctx.moveTo(0, level)
    for (let x = 0; x <= 240; x++) {
      const speed = isHyper.value ? 0.1 : 0.04
      const amplitude = isHyper.value ? 15 : 8
      const y = level + Math.sin(x * speed + t) * amplitude + Math.cos(x * 0.01 + t * 0.5) * 4
      ctx.lineTo(x, y)
    }
    ctx.lineTo(240, 300); ctx.lineTo(0, 300)
    ctx.fill()
    
    // 深渊之眼逻辑 (低概率出现)
    if (eyeLife <= 0 && Math.random() < 0.005) {
      eyeX = 40 + Math.random() * 160
      eyeY = level + 30 + Math.random() * 100
      eyeLife = 60 // 存活帧数
    }

    if (eyeLife > 0) {
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

    t += isHyper.value ? 0.15 : 0.06 
    requestAnimationFrame(animate)
  }
  animate()

  updateTimer()
  setInterval(updateTimer, 1000)
})
</script>

<style>
/* --- 全局：羊皮纸底色与噪音滤镜 --- */
:root { background: #F7F5F0; }
body { margin: 0; padding: 0; background: #F7F5F0; font-family: "Cinzel", serif; -webkit-font-smoothing: antialiased; overflow-x: hidden; }

.bg-noise { position: fixed; inset: 0; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="noiseFilter"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="3" stitchTiles="stitch"/></filter><rect width="100" height="100" filter="url(%23noiseFilter)"/></svg>'); opacity: 0.03; pointer-events: none; z-index: 999; }

.world { min-height: 100vh; display: flex; flex-direction: column; align-items: center; padding: 40px; color: #2C2C2C; transition: all 0.5s ease; }

/* 狂化模式 (200mg触发) */
.hyper-mode { filter: saturate(1.5) contrast(1.1); }
.hyper-mode .percentage { color: #8B0000; text-shadow: 2px 2px 0 rgba(139,0,0,0.2); }
.hyper-mode .main-title { animation: shake 0.5s infinite; }

/* 故障模式 (真名/世界观触发) */
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

/* 核心布局 */
.distorted-containment { display: flex; align-items: center; gap: 50px; margin-bottom: 60px; z-index: 10; relative; }

.fragmented-bottle { position: relative; width: 220px; height: 320px; }
.ink-content { width: 100%; height: 100%; background: #fff; box-shadow: 0 20px 50px rgba(0,0,0,0.1); overflow: hidden; -webkit-clip-path: url(#fractureShape); clip-path: url(#fractureShape); filter: contrast(1.1) brightness(1.02) blur(0.2px) hue-rotate(-5deg); transition: filter 0.3s; }
.fragmented-bottle:hover .ink-content { filter: contrast(1.2) brightness(1.05) blur(0.5px) hue-rotate(5deg); }
#ink { position: absolute; bottom: 0; left: 0; width: 100%; height: 100%; object-fit: cover; }
.bottle-frame { position: absolute; inset: -5px; border: 2px dashed #2C2C2C; -webkit-clip-path: url(#fractureShape); clip-path: url(#fractureShape); opacity: 0.2; pointer-events: none; }

/* 因果面板 */
.goal-panel { border: 1px solid rgba(44, 44, 44, 0.1); background: rgba(255,255,255,0.3); padding: 25px 30px; border-radius: 2px; text-align: left; position: relative; width: 320px; box-sizing: border-box; font-family: sans-serif; }
.goal-panel::before { content: ''; position: absolute; top: -10px; left: 10px; width: 20px; height: 2px; background: rgba(0,0,0,0.5); }
.goal-panel::after { content: ''; position: absolute; bottom: -1px; right: 10px; width: 2px; height: 10px; background: rgba(0,0,0,0.5); }

.panel-header { font-size: 10px; letter-spacing: 3px; opacity: 0.3; margin-bottom: 20px; text-transform: uppercase; font-weight: bold; }
.goal-display { position: relative; margin-bottom: 15px; }
.goal-prefix { font-size: 11px; letter-spacing: 1px; opacity: 0.2; position: absolute; top: 0; left: 0; }
.percentage { font-size: 5rem; font-weight: 900; line-height: 1; letter-spacing: -6px; margin-left: -5px; color: #1A1A1A; transition: color 0.3s; }
.percent-sign { font-size: 1.5rem; letter-spacing: 0; vertical-align: super; opacity: 0.3; margin-left: 5px; }

.panel-status { font-size: 11px; font-family: monospace; letter-spacing: 1px; margin-bottom: 8px; font-weight: bold; color: rgba(0,0,0,0.8); }
.panel-flavor { font-size: 9px; opacity: 0.5; margin: 0 0 15px 0; font-family: sans-serif; line-height: 1.4; color: #8B0000; }

.sub-targets { display: flex; gap: 8px; }
.tag { font-size: 8px; padding: 3px 6px; border: 1px solid rgba(0,0,0,0.2); border-radius: 2px; opacity: 0.6; letter-spacing: 1px; }

/* 底部修齐台 */
.altar { margin-top: auto; width: 450px; text-align: center; border-bottom: 1px solid rgba(0,0,0,0.1); margin-bottom: 40px; z-index: 10; position: relative; }
input { width: 100%; background: none; border: none; outline: none; padding: 20px; text-align: center; font-style: italic; font-size: 1.2rem; color: #2C2C2C; font-family: sans-serif; letter-spacing: 1px; transition: all 0.3s; }
input::placeholder { color: rgba(0,0,0,0.2); font-style: normal; }
input:focus { background: rgba(0,0,0,0.02); }
.altar-stats { display: flex; justify-content: space-between; padding: 12px 10px; font-size: 0.6rem; opacity: 0.5; letter-spacing: 2px; font-family: monospace; text-transform: uppercase; }

/* 动画定义 */
@keyframes shake { 
  0% { transform: translate(1px, 1px) rotate(0deg); } 
  25% { transform: translate(-1px, -2px) rotate(-1deg); } 
  50% { transform: translate(-3px, 0px) rotate(1deg); } 
  75% { transform: translate(3px, 2px) rotate(0deg); } 
  100% { transform: translate(1px, -1px) rotate(0deg); } 
}
@keyframes glitch-anim {
  0% { clip: rect(10px, 9999px, 30px, 0); }
  20% { clip: rect(50px, 9999px, 60px, 0); }
  40% { clip: rect(20px, 9999px, 40px, 0); }
  60% { clip: rect(70px, 9999px, 80px, 0); }
  80% { clip: rect(5px, 9999px, 15px, 0); }
  100% { clip: rect(35px, 9999px, 50px, 0); }
}
@keyframes pulse-border {
  0% { border-color: rgba(139, 0, 0, 0.2); }
  50% { border-color: rgba(139, 0, 0, 0.6); }
  100% { border-color: rgba(139, 0, 0, 0.2); }
}
</style>