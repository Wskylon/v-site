<template>
  <div class="void" :class="{ 'hyper': isHyper }">
    <div class="doom-header">
      <div class="brand">
        <h1 class="main-title">{{ pageTitle }}</h1>
        <p class="tagline">LOGIC_CORRUPTOR // 万象界</p>
      </div>
      <div class="timer-box">
        <div class="big-clock">{{ timer }}</div>
        <div class="sub-label">TARGET: 2029 USTC 22408</div>
      </div>
    </div>

    <div class="manifest-area">
      <div class="vessel-wrapper">
        <div class="vessel">
          <canvas id="ink"></canvas>
        </div>
        <div class="vessel-deco"></div>
      </div>

      <div class="data-panel">
        <div class="log-title">GOAL_ASSIMILATION_LOG</div>
        <div class="big-percent">
          <span class="num">{{ wordProgress }}</span>
          <span class="unit">%</span>
        </div>
        <div class="status-line">STATUS: {{ systemStatus }}</div>
        <div class="status-line">KARMA: {{ karmaValue }}</div>
        <div class="flavor-text">> {{ flavorText }}</div>
      </div>
    </div>

    <div class="altar-zone">
      <div class="input-wrap">
        <input 
          v-model="insight" 
          @keyup.enter="handleCommand" 
          placeholder="ENTER TRUE NAME OR DOSE..." 
        />
      </div>
      <div class="footer-stats">
        <span>GYM: 3-ON-2-OFF</span>
        <span>CAFFEINE: {{ currentCaffeine }}MG</span>
        <span>ZHUANSHENGBEN: 2027</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const timer = ref('000D | 00:00:00')
const wordProgress = ref(50)
const insight = ref('')
const pageTitle = ref("V'ZUOR KHAA-SH'AN")
const systemStatus = ref('IDLE')
const flavorText = ref('因果漂移检测正常')
const currentCaffeine = ref(200)
const karmaValue = ref(15)
const isHyper = ref(false)

const updateTimer = () => {
  const target = new Date('2028-12-23T08:30:00').getTime()
  const diff = target - new Date().getTime()
  const d = Math.floor(diff / 86400000)
  const h = Math.floor((diff / 3600000) % 24).toString().padStart(2,'0')
  const m = Math.floor((diff / 60000) % 60).toString().padStart(2,'0')
  const s = Math.floor((diff / 1000) % 60).toString().padStart(2,'0')
  timer.value = `${d}D | ${h}:${m}:${s}`
}

const handleCommand = () => {
  const cmd = insight.value.trim().toLowerCase()
  if (cmd === '200mg') {
    isHyper.value = true; currentCaffeine.value = 400
    systemStatus.value = 'OVERLOAD'; flavorText.value = '警告：神经元强制同步中'
    setTimeout(() => { isHyper.value = false; currentCaffeine.value = 200; systemStatus.value = 'IDLE' }, 5000)
  }
  insight.value = ''
}

onMounted(() => {
  const cvs = document.getElementById('ink'); const ctx = cvs.getContext('2d')
  cvs.width = 250; cvs.height = 350; let t = 0
  function animate() {
    ctx.clearRect(0, 0, 250, 350)
    ctx.fillStyle = isHyper.value ? '#AA0000' : '#111111'
    const level = 350 * (1 - wordProgress.value / 100)
    ctx.beginPath(); ctx.moveTo(0, level)
    for (let x = 0; x <= 250; x++) {
      ctx.lineTo(x, level + Math.sin(x * 0.05 + t) * (isHyper.value ? 12 : 6))
    }
    ctx.lineTo(250, 350); ctx.lineTo(0, 350); ctx.fill()
    // 强制显示一个明显的红眼
    if (Math.sin(t*0.5) > 0.8) {
      ctx.fillStyle = 'red'; ctx.beginPath(); ctx.arc(125, level + 50, 4, 0, 7); ctx.fill()
    }
    t += 0.08; requestAnimationFrame(animate)
  }
  animate(); setInterval(updateTimer, 1000)
})
</script>

<style>
/* 放弃依赖外部字体，改用强力的系统衬线体 */
body { 
  margin: 0; padding: 0; background: #E8E4D9; 
  font-family: "Garamond", "Georgia", serif; 
  color: #1a1a1a; overflow: hidden;
}
.void { 
  height: 100vh; display: flex; flex-direction: column; 
  padding: 40px; box-sizing: border-box; transition: background 0.3s;
}
.hyper { background: #000 !important; color: #ff3333 !important; }

/* 头部布局 */
.doom-header { 
  display: flex; justify-content: space-between; align-items: flex-start;
  border-bottom: 2px solid #1a1a1a; padding-bottom: 20px;
}
.main-title { font-size: 2.5rem; margin: 0; font-weight: 900; letter-spacing: 1px; }
.tagline { margin: 5px 0 0 0; font-size: 0.8rem; letter-spacing: 5px; opacity: 0.6; }
.big-clock { font-size: 2.5rem; font-weight: 900; font-family: monospace; }
.sub-label { font-size: 0.7rem; text-align: right; opacity: 0.5; }

/* 中部核心区 */
.manifest-area { 
  flex: 1; display: flex; align-items: center; justify-content: center; gap: 80px; 
}
.vessel-wrapper { position: relative; }
.vessel { 
  width: 220px; height: 320px; background: #fff; 
  border: 3px solid #1a1a1a;
  clip-path: polygon(10% 0, 90% 0, 100% 40%, 85% 100%, 15% 100%, 0 40%);
}
.data-panel { text-align: left; min-width: 300px; }
.log-title { font-size: 0.7rem; letter-spacing: 3px; border-bottom: 1px solid #1a1a1a; padding-bottom: 10px; margin-bottom: 20px; }
.big-percent { font-size: 8rem; line-height: 0.8; font-weight: 900; margin-left: -10px; }
.percent-sign { font-size: 2rem; opacity: 0.3; }
.status-line { font-family: monospace; font-size: 0.9rem; margin-top: 10px; font-weight: bold; }
.flavor-text { font-size: 0.8rem; margin-top: 20px; color: #8b0000; font-style: italic; }

/* 底部修齐台 */
.altar-zone { border-top: 2px solid #1a1a1a; padding-top: 20px; }
.input-wrap { width: 100%; border-bottom: 1px solid rgba(0,0,0,0.1); }
input { 
  width: 100%; background: none; border: none; outline: none; 
  padding: 15px; text-align: center; font-size: 1.5rem; font-family: inherit;
  color: inherit;
}
.footer-stats { display: flex; justify-content: space-between; padding: 10px; font-size: 0.7rem; font-family: monospace; opacity: 0.6; }

/* 响应式微调 */
@media (max-width: 768px) {
  .manifest-area { flex-direction: column; gap: 30px; }
  .big-percent { font-size: 5rem; }
  .doom-header { flex-direction: column; }
}
</style>