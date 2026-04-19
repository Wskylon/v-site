<template>
  <div class="world" :class="{ 'glitch-mode': isGlitching, 'hyper-mode': isHyper }">
    <header class="top-bar">
      <div class="title-group">
        <h1 class="main-title">{{ pageTitle }}</h1>
        <p class="subtitle">底层逻辑篡改者 · 万象界</p>
      </div>
      <div class="countdown-group">
        <div class="clock">{{ timer }}</div>
        <p class="clock-label">2029 USTC 22408 DOOMSDAY</p>
      </div>
    </header>

    <main class="distorted-containment">
      <div class="fragmented-bottle">
        <div class="ink-content">
          <canvas id="ink"></canvas>
        </div>
      </div>

      <aside class="goal-panel">
        <div class="panel-header">GOAL_ASSIMILATION_LOG</div>
        <div class="goal-display">
          <div class="percentage">{{ wordProgress }}<span class="percent-sign">%</span></div>
        </div>
        <div class="panel-status">STATUS: {{ systemStatus }}</div>
        <p class="panel-flavor">{{ flavorText }}</p>
      </aside>
    </main>

    <div class="altar">
      <input v-model="insight" @keyup.enter="handleCommand" :placeholder="inputPlaceholder" />
      <div class="altar-stats">
        <span>DOJO: 3-ON-2-OFF</span>
        <span>CAFFEINE: {{ currentCaffeine }}MG</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const timer = ref('载入中...')
const wordProgress = ref(50) 
const insight = ref('')
const pageTitle = ref("V'zuor Khaa-Sh'an")
const systemStatus = ref('SIPHONING :: IDLE')
const flavorText = ref('因果漂移检测：正常 / 逻辑污染：低')
const inputPlaceholder = ref('输入今日悟道所得...')
const currentCaffeine = ref(200)

const isGlitching = ref(false)
const isHyper = ref(false)

const updateTimer = () => {
  const target = new Date('2028-12-23T08:30:00').getTime() 
  const now = new Date().getTime()
  const diff = target - now
  if (diff <= 0) { timer.value = "因果已至"; return }
  const d = Math.floor(diff / (1000 * 60 * 60 * 24))
  const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
  const s = Math.floor((diff % (1000 * 60)) / 1000)
  timer.value = `${d}D | ${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`
}

const handleCommand = () => {
  const cmd = insight.value.trim().toLowerCase()
  if (cmd === '200mg') {
    isHyper.value = true
    currentCaffeine.value = 400
    setTimeout(() => { isHyper.value = false; currentCaffeine.value = 200 }, 3000)
  } else if (cmd === 'vzuor') {
    isGlitching.value = true
    setTimeout(() => isGlitching.value = false, 2000)
  }
  insight.value = ''
}

onMounted(() => {
  const cvs = document.getElementById('ink')
  const ctx = cvs.getContext('2d')
  cvs.width = 240; cvs.height = 300
  let t = 0
  function animate() {
    ctx.clearRect(0, 0, 240, 300)
    ctx.fillStyle = isHyper.value ? '#8B0000' : '#1A1A1A'
    const level = 300 * (1 - wordProgress.value / 100)
    ctx.beginPath()
    ctx.moveTo(0, level)
    for (let x = 0; x <= 240; x++) {
      ctx.lineTo(x, level + Math.sin(x * 0.04 + t) * (isHyper.value ? 15 : 8))
    }
    ctx.lineTo(240, 300); ctx.lineTo(0, 300)
    ctx.fill()
    t += 0.06
    requestAnimationFrame(animate)
  }
  animate()
  updateTimer()
  setInterval(updateTimer, 1000)
})
</script>

<style>
body { margin: 0; padding: 0; background: #F7F5F0; font-family: serif; color: #2C2C2C; overflow-x: hidden; }
.world { min-height: 100vh; display: flex; flex-direction: column; align-items: center; padding: 40px; }
.top-bar { width: 100%; max-width: 900px; display: flex; justify-content: space-between; margin-bottom: 80px; }
.main-title { font-size: 2rem; margin: 0; font-weight: bold; }
.clock { font-size: 2.2rem; font-weight: bold; font-family: monospace; }
.distorted-containment { display: flex; align-items: center; gap: 40px; margin-bottom: 60px; }
.fragmented-bottle { width: 200px; height: 300px; border: 2px solid #2C2C2C; border-radius: 0 0 80px 80px; overflow: hidden; background: #fff; }
.ink-content { width: 100%; height: 100%; position: relative; }
.goal-panel { border-left: 3px solid #1A1A1A; padding-left: 20px; text-align: left; width: 280px; }
.percentage { font-size: 5rem; font-weight: 900; letter-spacing: -4px; line-height: 1; }
.altar { margin-top: auto; width: 400px; border-bottom: 1px solid #ccc; margin-bottom: 40px; }
input { width: 100%; background: none; border: none; outline: none; padding: 15px; text-align: center; font-style: italic; font-size: 1.2rem; }
.altar-stats { display: flex; justify-content: space-between; font-size: 0.7rem; opacity: 0.5; padding: 10px; }
.hyper-mode { filter: saturate(2) contrast(1.2); }
</style>