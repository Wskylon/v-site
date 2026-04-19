<template>
  <div class="world">
    <header class="top-bar">
      <div class="title-group">
        <h1 class="main-title">V'zuor Khaa-Sh'an</h1>
        <p class="subtitle">底层逻辑篡改者 · 雾锁空山</p>
      </div>
      <div class="countdown-group">
        <div class="clock">{{ timer }}</div>
        <p class="clock-label">22408 科软 / 2027 专升本</p>
      </div>
    </header>

    <div class="center-content">
      <div class="bottle">
        <canvas id="ink"></canvas>
        <div class="water-text">{{ wordProgress }}%</div>
      </div>
      <div class="bottle-label">WORD SIPHONING</div>
    </div>

    <div class="altar">
      <input v-model="insight" @keyup.enter="submitInsight" placeholder="输入今日悟道所得..." />
      <div class="altar-stats">
        <span>FOCUS: 5.5H</span>
        <span>CAFFEINE: 200MG</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const timer = ref('00:00:00')
const wordProgress = ref(45) 
const insight = ref('')

onMounted(() => {
  const cvs = document.getElementById('ink')
  const ctx = cvs.getContext('2d')
  cvs.width = 200; cvs.height = 300
  let t = 0
  function animate() {
    ctx.clearRect(0, 0, 200, 300)
    ctx.fillStyle = '#2C2C2C'
    const h = 300 * (1 - wordProgress.value / 100)
    ctx.beginPath()
    ctx.moveTo(0, h)
    for (let x = 0; x <= 200; x++) {
      ctx.lineTo(x, h + Math.sin(x * 0.03 + t) * 5)
    }
    ctx.lineTo(200, 300); ctx.lineTo(0, 300)
    ctx.fill()
    t += 0.05
    requestAnimationFrame(animate)
  }
  animate()

  setInterval(() => {
    timer.value = new Date().toLocaleTimeString('zh-CN', { hour12: false })
  }, 1000)
})

const submitInsight = () => {
  if(insight.value.toLowerCase() === 'vzuor') {
    alert('真名契约已达成：欢迎回来，造物主。')
  }
  insight.value = ''
}
</script>

<style>
/* 强制全屏羊皮纸底色 */
:root { background: #F7F5F0; }
body { margin: 0; padding: 0; background: #F7F5F0; font-family: "Noto Serif SC", serif; }
.world { min-height: 100vh; display: flex; flex-direction: column; align-items: center; padding: 40px; color: #2C2C2C; }
.top-bar { width: 100%; max-width: 800px; display: flex; justify-content: space-between; margin-bottom: 80px; }
.main-title { font-size: 2.2rem; margin: 0; letter-spacing: 2px; }
.subtitle { font-size: 0.7rem; opacity: 0.5; margin: 5px 0; letter-spacing: 4px; text-transform: uppercase; }
.clock { font-size: 2.2rem; font-weight: bold; font-family: monospace; }
.clock-label { font-size: 0.6rem; opacity: 0.4; margin: 0; text-align: right; }
.bottle { width: 180px; height: 280px; border: 3px solid #2C2C2C; border-radius: 0 0 90px 90px; position: relative; overflow: hidden; background: #fff; box-shadow: 0 10px 30px rgba(0,0,0,0.05); }
#ink { position: absolute; bottom: 0; left: 0; }
.water-text { position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; font-size: 3rem; color: white; mix-blend-mode: difference; }
.bottle-label { margin-top: 15px; font-size: 0.7rem; letter-spacing: 5px; opacity: 0.3; text-align: center; }
.altar { margin-top: 60px; width: 320px; text-align: center; border-bottom: 1px solid rgba(0,0,0,0.1); }
input { width: 100%; background: none; border: none; outline: none; padding: 15px; text-align: center; font-style: italic; font-size: 1.1rem; color: #2C2C2C; }
.altar-stats { display: flex; justify-content: space-between; padding: 10px 0; font-size: 0.6rem; opacity: 0.4; letter-spacing: 1px; }
</style>