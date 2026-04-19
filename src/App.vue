<template>
  <div class="void-realm" :class="{ 'hyper-drive': isHyper, 'sanity-loss': isSanityLoss }">
    <div class="void-background">
      <div class="void-noise"></div>
      <div class="void-scanlines"></div>
    </div>

    <div class="interface-grid">
      
      <header class="oracle-header">
        <div class="oracle-left">
          <h1 class="arcane-title" :data-text="pageTitle">{{ pageTitle }}</h1>
          <div class="status-monitor">
            <span class="pulse-sig"></span>
            <span class="mono-label">KERN_V3.0 // ABYSSAL_ALTAR // [ {{ systemStatus }} ]</span>
          </div>
        </div>
        <div class="oracle-right">
          <div class="doom-clock">{{ timer }}</div>
          <p class="mission-tag">DESTINY_RECKONING: 2029_USTC_CS_22408</p>
        </div>
      </header>

      <main class="manifest-core">
        
        <div class="altar-unit">
          <svg width="0" height="0">
            <defs>
              <clipPath id="abyssalShape" clipPathUnits="objectBoundingBox">
                <path d="M 0.12 0.05 L 0.88 0 L 1 0.45 L 0.9 0.98 L 0.1 1 L 0 0.55 Z"></path>
              </clipPath>
            </defs>
          </svg>
          <div class="containment-vessel">
            <canvas id="abyss-canvas"></canvas>
            <div class="glass-glare"></div>
            <div class="space-distortion"></div>
          </div>
          <div class="altar-deco"></div>
        </div>

        <aside class="data-terminal">
          <div class="terminal-header">ASSIMILATION_LOG // 0x408D</div>
          
          <div class="massive-readout">
            <span class="readout-val">{{ wordProgress }}</span>
            <span class="readout-unit">%</span>
          </div>

          <div class="system-matrix">
            <div class="matrix-row">
              <span class="mono-key">KARMA_LOAD:</span>
              <span class="mono-val text-glitch" :data-text="karmaValue">{{ karmaValue }}</span>
            </div>
            <div class="matrix-row">
              <span class="mono-key">CAFFEINE_LVL:</span>
              <span class="mono-val" :class="{'c-high': isHyper}">{{ currentCaffeine }}MG</span>
            </div>
            <div class="matrix-row">
              <span class="mono-key">COGNITION:</span>
              <div class="tag-row mono-val">
                <span class="tech-tag">[ C_LANG ]</span>
                <span class="tech-tag">[ DATA_STRUCT ]</span>
              </div>
            </div>
          </div>

          <div class="console-box">
            <div class="whisper">> {{ flavorText }}</div>
            <div class="whisper blink-cursor">_</div>
          </div>
        </aside>
      </main>

      <footer class="console-sect">
        <div class="input-wrap">
          <input 
            class="abyssal-input"
            v-model="insight" 
            @keyup.enter="handleCommand" 
            placeholder="ENTER THE TRUE NAME OR THE INK COMMENCES..." 
            spellcheck="false"
          />
        </div>
        <div class="telemetry-bar">
          <div class="t-item">DOJO_CYCLE: 3-ON / 2-OFF</div>
          <div class="t-item text-red">ZHUANSHENGBEN_ANCHOR: 2027</div>
          <div class="t-item">WAN_XIANG_REALM // SECURED</div>
        </div>
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

// --- 状态变量 (由最大算力驱动) ---
const timer = ref('000D | 00:00:00')
const insight = ref('')
const pageTitle = ref("V'ZUOR KHAA-SH'AN")
const systemStatus = ref('SIPHONING')
const flavorText = ref('等待意志接入以维持祭坛水位。深渊检测到轻微业障波动。')
const currentCaffeine = ref(200)
const karmaValue = ref(23.7)
const isHyper = ref(false)
const isSanityLoss = ref(false)

// 数据持久化
const wordProgress = ref(Number(localStorage.getItem('vzuor_progress_v3')) || 55)
watch(wordProgress, (nv) => localStorage.setItem('vzuor_progress_v3', nv))

// --- 倒计时重载 ---
const updateTimer = () => {
  const target = new Date('2028-12-23T08:30:00').getTime()
  const diff = target - new Date().getTime()
  if (diff <= 0) { timer.value = "DESTINY_FULFILLED"; return }
  const d = Math.floor(diff / 86400000).toString().padStart(3,'0')
  const h = Math.floor((diff / 3600000) % 24).toString().padStart(2,'0')
  const m = Math.floor((diff / 60000) % 60).toString().padStart(2,'0')
  const s = Math.floor((diff / 1000) % 60).toString().padStart(2,'0')
  timer.value = `${d}D | ${h}:${m}:${s}`
}

// --- 指令处理器 (最大算力优化版) ---
const handleCommand = () => {
  const cmd = insight.value.trim().toLowerCase()
  if (!cmd) return

  if (cmd === '200mg' || cmd === 'dose') {
    triggerHyper()
  } else if (cmd === 'vzuor' || cmd === 'truename') {
    triggerSanityLoss()
  } else if (cmd === '+1' || cmd === 'word++') {
    wordProgress.value = Math.min(100, wordProgress.value + 1)
    karmaValue.value -= 0.5
    flavorText.value = '知识同化完成。本地业障降低。'
  } else {
    karmaValue.value += Math.random() * 2
    flavorText.value = `未知变量注入 [${cmd}]。深渊投来冰冷的目光。`
  }
  insight.value = ''
}

const triggerHyper = () => {
  isHyper.value = true; currentCaffeine.value = 400
  systemStatus.value = 'OVERDRIVE'; flavorText.value = '神经元强制过载。血月即将来临。'
  setTimeout(() => { isHyper.value = false; currentCaffeine.value = 200; systemStatus.value = 'SIPHONING' }, 6000)
}

const triggerSanityLoss = () => {
  isSanityLoss.value = true
  pageTitle.value = "H I D E  Y O U R S E L F"
  flavorText.value = '“你以为这是你的第一次考研吗？看看你手腕上的刻痕。”'
  setTimeout(() => { isSanityLoss.value = false; pageTitle.value = "V'ZUOR KHAA-SH'AN" }, 4000)
}

// --- Canvas 湍流粒子渲染引擎 ---
onMounted(() => {
  const cvs = document.getElementById('abyss-canvas')
  const ctx = cvs.getContext('2d')
  cvs.width = 240; cvs.height = 360
  
  let t = 0
  let eye = null

  function animate() {
    ctx.clearRect(0, 0, 240, 360)
    
    // 墨水颜色根据模式切换 (高狂模式变深红)
    ctx.fillStyle = isHyper.value ? '#660a0a' : '#10151a' 
    
    const targetH = 360 * (1 - wordProgress.value / 100)
    
    ctx.beginPath(); ctx.moveTo(0, targetH)
    for (let x = 0; x <= 240; x++) {
      const spd = isHyper.value ? 0.12 : 0.06
      const amp = isHyper.value ? 14 : 7
      const noise = isHyper.value ? Math.random() * 2 : 0 // 狂化时增加随机噪音
      ctx.lineTo(x, targetH + Math.sin(x * spd + t) * amp + noise)
    }
    ctx.lineTo(240, 360); ctx.lineTo(0, 360); ctx.fill()
    
    // --- 深渊之眼逻辑 V2.0 (Alpha 渐变) ---
    if (!eye && Math.random() < 0.005) {
      eye = { x: 50 + Math.random() * 140, y: targetH + 50 + Math.random() * 120, life: 100, maxLife: 100 }
    }
    if (eye) {
      const op = Math.sin((eye.life / eye.maxLife) * Math.PI) // 淡入淡出
      ctx.fillStyle = `rgba(180, 0, 0, ${op})`; ctx.beginPath(); ctx.ellipse(eye.x, eye.y, 8, 3, 0, 0, Math.PI * 2); ctx.fill() // 眼白
      ctx.fillStyle = `rgba(0, 0, 0, ${op})`; ctx.beginPath(); ctx.arc(eye.x, eye.y, 2 + Math.sin(t) * 0.5, 0, Math.PI * 2); ctx.fill() // 瞳孔颤动
      eye.life--; if (eye.life <= 0) eye = null
    }

    t += isHyper.value ? 0.18 : 0.08; requestAnimationFrame(animate)
  }
  animate(); setInterval(updateTimer, 1000)
})
</script>

<style>
/* 终极字体实装：装饰型 + 控制型 */
@import url('https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@700;900&family=Cinzel:wght@800&family=Space+Mono:wght@400;700&display=swap');

:root {
  --bg-parchment: #F2EFE8; /* 腐烂羊皮纸基色 */
  --fg-ink: #10151a;       /* 深渊墨水色 */
  --red-call: #C41C1C;     /* 古神召唤红 */
  --text-muted: #737067;   /* 尘封色 */
  
  --font-title: 'Cinzel Decorative', serif;
  --font-serif: 'Cinzel', serif;
  --font-mono: 'Space Mono', monospace;
}

body { margin: 0; background: var(--bg-parchment); color: var(--fg-ink); font-family: var(--font-serif); overflow: hidden; -webkit-font-smoothing: antialiased; }

/* 噪音干扰图层 */
.void-background { position: fixed; inset: 0; z-index: 0; pointer-events: none; }
.void-noise { position: absolute; inset: 0; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="3" stitchTiles="stitch"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.03; }
.void-scanlines { position: absolute; inset: 0; background: linear-gradient(rgba(0,0,0,0) 50%, rgba(0,0,0,0.02) 50%); background-size: 100% 4px; }

/* 终极狂化与故障模式视觉 */
.hyper-drive { background: #0A0505 !important; --bg-parchment: #0A0505; --fg-ink: #F2EFE8; }
.hyper-drive .containment-vessel { box-shadow: inset 0 10px 40px rgba(0,0,0,0.3), 0 0 30px rgba(196, 28, 28, 0.4); border-color: var(--red-call); }
.hyper-drive .arcane-title { color: var(--red-call); text-shadow: 0 0 15px rgba(196,28,28,0.6); }

.sanity-loss .void-background { color: #ff0000; animation: sanity-shake 0.3s infinite; }
.sanity-loss .arcane-title { animation: title-glitch 0.2s infinite; }
.sanity-loss .containment-vessel { border-color: white !important; }

.interface-grid {
  width: 100%; max-width: 1200px; height: 100vh;
  margin: 0 auto; padding: 60px; box-sizing: border-box;
  display: flex; flex-direction: column; gap: 60px;
  position: relative; z-index: 10;
}

/* 顶部：Oracle Header */
.oracle-header {
  display: flex; justify-content: space-between; align-items: flex-end;
  border-bottom: 2px solid var(--fg-ink); padding-bottom: 25px;
}
.arcane-title { font-family: var(--font-title); font-size: 2.8rem; margin: 0; letter-spacing: 2px; text-transform: uppercase; font-weight: 900; position: relative; }
.status-monitor { display: flex; align-items: center; gap: 10px; margin-top: 10px; opacity: 0.7; }
.pulse-sig { width: 8px; height: 8px; background: var(--red-call); border-radius: 50%; animation: pulse-anim 2s infinite; }
.mono-label { font-family: var(--font-mono); font-size: 0.7rem; font-weight: 700; letter-spacing: 1px; }

.doom-clock { font-size: 3rem; font-weight: 800; font-family: 'Courier New', monospace; letter-spacing: -3px; line-height: 1; }
.mission-tag { font-family: var(--font-mono); font-size: 0.7rem; color: var(--text-muted); margin: 5px 0 0 0; text-align: right; letter-spacing: 1px; }

/* 核心核心区：祭坛布局 */
.manifest-core { flex: 1; display: flex; align-items: center; gap: 100px; position: relative; }

/* 畸变祭坛容器 */
.containment-vessel {
  width: 240px; height: 360px;
  border: 4px solid var(--fg-ink);
  background: white;
  clip-path: url(#abyssalShape); /* 应用 SVG 碎片裁剪 */
  position: relative; overflow: hidden;
  box-shadow: inset 0 15px 50px rgba(0,0,0,0.1), 0 20px 40px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}
#abyss-canvas { position: absolute; bottom: 0; left: 0; width: 100%; height: 100%; }
.glass-glare { position: absolute; top: 0; left: 20%; width: 30%; height: 100%; background: linear-gradient(to right, rgba(255,255,255,0.2), transparent); transform: skewX(-15deg); }
.space-distortion {
  position: absolute; inset: 0; z-index: 5;
  backdrop-filter: blur(1px); /* 空间畸变效果 */
  mask-image: linear-gradient(to bottom, rgba(0,0,0,0.3) 10%, rgba(0,0,0,0) 50%);
}
.altar-deco { position: absolute; inset: -5px; border: 2px dashed var(--fg-ink); clip-path: url(#abyssalShape); opacity: 0.2; pointer-events: none; }

/* 数据终端面板 */
.data-terminal { flex: 1; text-align: left; }
.terminal-header { font-family: var(--font-mono); font-size: 0.75rem; border-bottom: 1px solid rgba(0,0,0,0.1); padding-bottom: 10px; margin-bottom: 30px; letter-spacing: 2px; opacity: 0.6; }

.massive-readout { margin-bottom: 40px; }
.readout-val { font-family: var(--font-title); font-size: 11rem; font-weight: 900; line-height: 0.75; letter-spacing: -8px; margin-left: -10px; }
.readout-unit { font-size: 2.5rem; color: var(--text-muted); margin-left: 10px; }

.system-matrix { font-family: var(--font-mono); display: flex; flex-direction: column; gap: 18px; margin-top: 40px; }
.matrix-row { display: flex; gap: 20px; align-items: center; font-size: 0.9rem; }
.mono-key { color: var(--text-muted); font-weight: 700; width: 120px; }
.mono-val { font-weight: 700; }
.c-high { color: var(--red-call); font-weight: 900; animation: caffeine-pulse 0.5s infinite; }
.tag-row { display: flex; gap: 10px; }
.tech-tag { font-size: 0.75rem; border: 1px solid rgba(0,0,0,0.2); padding: 2px 8px; border-radius: 3px; }

/* 终极呓语日志：腐烂羊皮纸质感 */
.console-box {
  margin-top: 50px; padding: 25px;
  background: rgba(0,0,0,0.03);
  border-left: 5px solid var(--fg-ink);
  font-family: var(--font-mono); font-size: 0.85rem; font-weight: 400; line-height: 1.6;
  position: relative;
}
/* SVG 羊皮纸腐烂效果滤镜引用 (略，通过 CSS 渐变模拟) */
.console-box::after { content: ''; position: absolute; inset: 0; background: linear-gradient(rgba(242, 239, 232, 0.2), rgba(242, 239, 232, 0) 70%); }

.console-txt, whisper { color: var(--red-call); }
.blink-cursor { animation: cursor-blink 1s step-end infinite; }

/* 底部修齐台：Console Altar */
.console-sect { border-top: 2px solid var(--fg-ink); }
.input-wrap { width: 100%; border-bottom: 1px solid rgba(0,0,0,0.05); }
.abyssal-input {
  width: 100%; background: none; border: none; outline: none;
  padding: 30px 0; font-family: var(--font-serif); font-size: 1.8rem;
  color: inherit; text-align: center; font-style: italic; letter-spacing: 1px;
}
.abyssal-input::placeholder { color: var(--text-muted); font-style: normal; opacity: 0.4; font-family: var(--font-mono); font-size: 1rem; }

.telemetry-bar { display: flex; justify-content: space-between; padding: 15px 0; font-family: var(--font-mono); font-size: 0.7rem; color: var(--text-muted); letter-spacing: 1px; font-weight: 700; }
.text-red { color: var(--red-call); }

/* --- 终极动画系统 --- */
@keyframes pulse-anim { 0% { opacity: 0.3; } 50% { opacity: 1; } 100% { opacity: 0.3; } }
@keyframes cursor-blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }
@keyframes title-glitch { 0% { transform: skewX(1deg); color: #ff0000; } 50% { transform: skewX(-1deg); color: #00ffff; } 100% { transform: skewX(0); } }
@keyframes sanity-shake { 0% { transform: translate(1px, 1px); } 100% { transform: translate(-1px, -1px); } }
@keyframes caffeine-pulse { 0%, 100% { opacity: 1; transform: scale(1); } 50% { opacity: 0.8; transform: scale(1.05); } }

/* 文本故障效果，V1.6 的 45% 就是缺少这个才显得幼稚 */
.text-glitch::before, .text-glitch::after { content: attr(data-text); position: absolute; top: 0; left: 0; opacity: 0.8; }
.hyper-drive .text-glitch { position: relative; }
.hyper-drive .text-glitch::before { animation: glitch-anim-1 0.4s infinite linear alternate-reverse; color: #ff0000; z-index: -1; }
.hyper-drive .text-glitch::after { animation: glitch-anim-2 0.3s infinite linear alternate-reverse; color: #00ffff; z-index: -2; }
@keyframes glitch-anim-1 { 0% { clip: rect(10px, 9999px, 30px, 0); } 100% { clip: rect(50px, 9999px, 80px, 0); } }
@keyframes glitch-anim-2 { 0% { clip: rect(30px, 9999px, 60px, 0); } 100% { clip: rect(10px, 9999px, 30px, 0); } }

/* 移动端绝对适配 (最大算力优化) */
@media (max-width: 950px) {
  .interface-grid { padding: 30px; gap: 40px; }
  .manifest-core { flex-direction: column; gap: 40px; align-items: center; }
  .doom-clock { font-size: 2rem; }
  .massive-readout { text-align: center; }
  .readout-val { font-size: 7rem; }
  .console-box { margin-top: 30px; }
  .abyssal-input { font-size: 1.4rem; }
}
</style>