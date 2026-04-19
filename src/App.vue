<template>
  <div class="void-realm" :class="{ 'hyper-state': isHyper, 'glitch-state': isGlitching }">
    <div class="scanlines"></div>

    <div class="interface-container">
      
      <header class="doom-header">
        <div class="brand-block">
          <h1 class="main-title" :data-text="pageTitle">{{ pageTitle }}</h1>
          <div class="sub-brand">
            <span class="pulse-dot"></span>
            SYS.OVERRIDE_ // 万象界底座
          </div>
        </div>
        <div class="timer-block">
          <div class="countdown-digits">{{ timer }}</div>
          <div class="target-label">DESTINY: 2029 USTC_22408 EXAM</div>
        </div>
      </header>

      <main class="manifest-core">
        
        <div class="containment-vessel">
          <div class="glass-cylinder">
            <canvas id="ink-canvas"></canvas>
            <div class="glass-reflection"></div>
          </div>
          <div class="vessel-base-plate">
            [ WORD_SIPHONING_UNIT ]
          </div>
        </div>

        <aside class="data-terminal">
          <div class="terminal-header">
            <span>ASSIMILATION_LOG</span>
            <span class="tracking-id">ID: 408-0x7F</span>
          </div>
          
          <div class="massive-readout">
            <span class="readout-value">{{ wordProgress }}</span>
            <span class="readout-unit">%</span>
          </div>

          <div class="status-matrix">
            <div class="matrix-row">
              <span class="label">SYS_STATUS:</span>
              <span class="value" :class="{'text-red': isHyper}">{{ systemStatus }}</span>
            </div>
            <div class="matrix-row">
              <span class="label">KARMA_LOAD:</span>
              <span class="value">{{ karmaValue }}</span>
            </div>
            <div class="matrix-row">
              <span class="label">MEM_SECTORS:</span>
              <span class="value tags">
                <span class="tech-tag">C-LANG</span>
                <span class="tech-tag">DATA-STRUCT</span>
              </span>
            </div>
          </div>

          <div class="flavor-console">
            <div class="console-line">> {{ flavorText }}</div>
            <div class="console-line blink-cursor">_</div>
          </div>
        </aside>
      </main>

      <footer class="altar-console">
        <input 
          class="command-input"
          v-model="insight" 
          @keyup.enter="handleCommand" 
          placeholder="ENTER DOSE (e.g., 200mg) OR COMMAND..." 
          spellcheck="false"
        />
        <div class="telemetry-data">
          <span>CYCLE: 3-ON-2-OFF</span>
          <span>CAFFEINE_LVL: {{ currentCaffeine }}MG</span>
          <span>DEATH_RETURN: INACTIVE</span>
        </div>
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

// --- 核心状态响应 ---
const timer = ref('000D | 00:00:00')
const insight = ref('')
const pageTitle = ref("V'ZUOR KHAA-SH'AN")
const systemStatus = ref('IDLE / MONITORING')
const flavorText = ref('因果漂移检测正常。等待意志注入。')
const currentCaffeine = ref(200)
const karmaValue = ref(15)

const isHyper = ref(false)
const isGlitching = ref(false)

// --- 数据持久化 (LocalStorage) ---
// 初始化时从本地读取进度，如果没有则默认为 50 (2000/4000)
const wordProgress = ref(Number(localStorage.getItem('vzuor_progress')) || 50)

// 监听进度变化并永久保存
watch(wordProgress, (newVal) => {
  localStorage.setItem('vzuor_progress', newVal)
})

// --- 倒计时逻辑 ---
const updateTimer = () => {
  const target = new Date('2028-12-23T08:30:00').getTime()
  const diff = target - new Date().getTime()
  if (diff <= 0) { timer.value = "DOOMSDAY ARRIVED"; return }
  const d = Math.floor(diff / 86400000).toString().padStart(3,'0')
  const h = Math.floor((diff / 3600000) % 24).toString().padStart(2,'0')
  const m = Math.floor((diff / 60000) % 60).toString().padStart(2,'0')
  const s = Math.floor((diff / 1000) % 60).toString().padStart(2,'0')
  timer.value = `${d}D | ${h}:${m}:${s}`
}

// --- 指令中枢 ---
const handleCommand = () => {
  const cmd = insight.value.trim().toLowerCase()
  if (!cmd) return

  if (cmd === '200mg') {
    triggerHyper()
  } else if (cmd === 'vzuor') {
    triggerGlitch()
  } else if (cmd === 'word++' || cmd === '+1') {
    // 允许用户通过指令增加进度
    wordProgress.value = Math.min(100, wordProgress.value + 1)
    flavorText.value = `词汇量已同步刻印。当前进度：${wordProgress.value}%`
    karmaValue.value -= 2
  } else {
    karmaValue.value += Math.floor(Math.random() * 4) + 1
    flavorText.value = `未知指令 [${cmd}]。业障值上升。`
  }
  insight.value = ''
}

const triggerHyper = () => {
  isHyper.value = true
  currentCaffeine.value = 400
  systemStatus.value = 'OVERLOAD / BLOOD_MOON'
  flavorText.value = '警告：神经元强制同步中。心率突破安全阈值。'
  setTimeout(() => { 
    isHyper.value = false
    currentCaffeine.value = 200
    systemStatus.value = 'IDLE / MONITORING'
    flavorText.value = '药效消退，系统恢复常规循环。'
  }, 6000)
}

const triggerGlitch = () => {
  isGlitching.value = true
  flavorText.value = '“三柱神已察觉你的苏醒。协议解除。”'
  setTimeout(() => isGlitching.value = false, 3000)
}

// --- Canvas 高性能渲染引擎 ---
onMounted(() => {
  const cvs = document.getElementById('ink-canvas')
  const ctx = cvs.getContext('2d')
  // 尺寸严格匹配 CSS 中的容器尺寸
  cvs.width = 240
  cvs.height = 360
  
  let t = 0
  let eyeData = null

  function animate() {
    ctx.clearRect(0, 0, 240, 360)
    
    // 颜色根据状态切换
    ctx.fillStyle = isHyper.value ? '#7A0000' : '#141414'
    
    // 水位计算
    const targetHeight = 360 * (1 - wordProgress.value / 100)
    
    ctx.beginPath()
    ctx.moveTo(0, targetHeight)
    
    // 绘制波浪
    for (let x = 0; x <= 240; x++) {
      const speed = isHyper.value ? 0.08 : 0.03
      const amp = isHyper.value ? 12 : 5
      const wave = Math.sin(x * speed + t) * amp + Math.cos(x * 0.02 + t * 0.8) * 3
      ctx.lineTo(x, targetHeight + wave)
    }
    ctx.lineTo(240, 360)
    ctx.lineTo(0, 360)
    ctx.fill()

    // --- 深渊之眼逻辑 ---
    if (!eyeData && Math.random() < 0.005) {
      eyeData = {
        x: 40 + Math.random() * 160,
        y: targetHeight + 50 + Math.random() * 100,
        life: 100,
        maxLife: 100
      }
    }

    if (eyeData) {
      const opacity = Math.sin((eyeData.life / eyeData.maxLife) * Math.PI) // 淡入淡出
      
      // 绘制眼白(血红)
      ctx.fillStyle = `rgba(180, 0, 0, ${opacity})`
      ctx.beginPath()
      ctx.ellipse(eyeData.x, eyeData.y, 10, 4, 0, 0, Math.PI * 2)
      ctx.fill()
      
      // 绘制瞳孔(漆黑)
      const pupilSize = 2 + Math.sin(t * 2) * 0.5 // 瞳孔颤动
      ctx.fillStyle = `rgba(0, 0, 0, ${opacity})`
      ctx.beginPath()
      ctx.arc(eyeData.x, eyeData.y, pupilSize, 0, Math.PI * 2)
      ctx.fill()

      eyeData.life--
      if (eyeData.life <= 0) eyeData = null
    }

    t += isHyper.value ? 0.12 : 0.05
    requestAnimationFrame(animate)
  }
  
  animate()
  updateTimer()
  setInterval(updateTimer, 1000)
})
</script>

<style>
/* 强制引入高品质双字体库 */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@500;800;900&family=JetBrains+Mono:wght@400;700&display=swap');

/* 全局重置与变量 */
:root {
  --bg-color: #EBE9E1;
  --text-main: #141414;
  --text-muted: #66635A;
  --accent-red: #A31D1D;
  --border-color: #1A1A1A;
  
  --font-serif: 'Cinzel', serif;
  --font-mono: 'JetBrains Mono', monospace;
}

body {
  margin: 0; padding: 0;
  background-color: var(--bg-color);
  color: var(--text-main);
  overflow-x: hidden;
}

/* 扫描线图层 */
.scanlines {
  position: fixed; inset: 0; z-index: 9999; pointer-events: none;
  background: linear-gradient(to bottom, rgba(255,255,255,0), rgba(255,255,255,0) 50%, rgba(0,0,0,0.02) 50%, rgba(0,0,0,0.02));
  background-size: 100% 4px;
}

.void-realm {
  min-height: 100vh;
  display: flex; justify-content: center;
  transition: background-color 0.4s ease;
}

/* 狂化状态样式 */
.hyper-state {
  --bg-color: #0A0505;
  --text-main: #EBE9E1;
  --border-color: #521111;
  --text-muted: #8C4040;
}
.hyper-state .massive-readout { color: var(--accent-red); text-shadow: 0 0 20px rgba(163,29,29,0.4); }
.hyper-state .main-title { animation: slight-shake 0.3s infinite; }

/* 容器居中限制 */
.interface-container {
  width: 100%; max-width: 1100px;
  padding: 50px 40px;
  display: flex; flex-direction: column;
}

/* --- 顶部 Header --- */
.doom-header {
  display: flex; justify-content: space-between; align-items: flex-end;
  border-bottom: 2px solid var(--border-color);
  padding-bottom: 20px; margin-bottom: 60px;
}
.main-title {
  font-family: var(--font-serif);
  font-size: 2.8rem; font-weight: 900;
  margin: 0; letter-spacing: 2px;
}
.sub-brand {
  font-family: var(--font-mono); font-size: 0.75rem;
  color: var(--text-muted); margin-top: 8px;
  display: flex; align-items: center; gap: 8px;
}
.pulse-dot {
  width: 6px; height: 6px; background-color: var(--accent-red);
  border-radius: 50%; animation: pulse 2s infinite;
}
.timer-block { text-align: right; }
.countdown-digits {
  font-family: var(--font-mono); font-size: 3.2rem; font-weight: 700;
  letter-spacing: -2px; line-height: 1;
}
.target-label {
  font-family: var(--font-mono); font-size: 0.7rem;
  color: var(--text-muted); margin-top: 5px; letter-spacing: 1px;
}

/* --- 中部核心区 --- */
.manifest-core {
  flex: 1; display: flex; align-items: center; gap: 80px;
  padding: 0 20px;
}

/* 培养皿容器 (完全重构) */
.containment-vessel { display: flex; flex-direction: column; align-items: center; gap: 15px; }
.glass-cylinder {
  width: 240px; height: 360px;
  border: 4px solid var(--border-color);
  /* 拱窗形物理边界，彻底解决溢出 */
  border-radius: 120px 120px 8px 8px; 
  background-color: rgba(255,255,255,0.05);
  position: relative; overflow: hidden;
  box-shadow: inset 0 10px 30px rgba(0,0,0,0.1), 0 20px 40px rgba(0,0,0,0.08);
}
#ink-canvas { position: absolute; bottom: 0; left: 0; width: 100%; height: 100%; }
.glass-reflection {
  position: absolute; top: 5%; left: 10%; width: 30%; height: 90%;
  background: linear-gradient(to right, rgba(255,255,255,0.1), transparent);
  border-radius: 100px; transform: skewX(-10deg); pointer-events: none;
}
.vessel-base-plate {
  font-family: var(--font-mono); font-size: 0.65rem; font-weight: 700;
  letter-spacing: 2px; color: var(--text-muted);
}

/* 数据剥离面板 */
.data-terminal {
  flex: 1; display: flex; flex-direction: column;
}
.terminal-header {
  font-family: var(--font-mono); font-size: 0.75rem; font-weight: 700;
  color: var(--text-muted); letter-spacing: 1px;
  border-bottom: 1px solid rgba(0,0,0,0.1); padding-bottom: 10px;
  display: flex; justify-content: space-between;
}
.hyper-state .terminal-header { border-bottom-color: rgba(255,255,255,0.1); }
.massive-readout { margin: 20px 0; }
.readout-value {
  font-family: var(--font-serif); font-size: 9rem; font-weight: 900;
  line-height: 0.8; letter-spacing: -5px; margin-left: -8px;
}
.readout-unit {
  font-family: var(--font-serif); font-size: 2.5rem; color: var(--text-muted);
}
.status-matrix {
  font-family: var(--font-mono); font-size: 0.85rem;
  display: flex; flex-direction: column; gap: 12px; margin-top: 20px;
}
.matrix-row { display: flex; gap: 20px; align-items: center; }
.label { color: var(--text-muted); width: 100px; }
.value { font-weight: 700; }
.text-red { color: var(--accent-red); }
.tags { display: flex; gap: 8px; }
.tech-tag {
  font-size: 0.65rem; padding: 2px 6px; 
  border: 1px solid var(--border-color); border-radius: 2px;
}
.flavor-console {
  margin-top: 40px; background: rgba(0,0,0,0.03); padding: 15px;
  border-left: 3px solid var(--border-color);
  font-family: var(--font-mono); font-size: 0.8rem;
  color: var(--text-muted); display: flex; gap: 5px;
}
.hyper-state .flavor-console { background: rgba(255,255,255,0.03); color: var(--accent-red); border-color: var(--accent-red); }
.blink-cursor { animation: blink 1s step-end infinite; }

/* --- 底部修齐台 --- */
.altar-console { margin-top: 60px; border-top: 2px solid var(--border-color); }
.command-input {
  width: 100%; box-sizing: border-box; background: transparent; border: none; outline: none;
  padding: 25px 0; font-family: var(--font-serif); font-size: 1.5rem; font-style: italic;
  color: var(--text-main); border-bottom: 1px solid rgba(0,0,0,0.05);
}
.command-input::placeholder { color: var(--text-muted); opacity: 0.5; font-style: normal; font-family: var(--font-mono); font-size: 1rem; }
.hyper-state .command-input { border-bottom-color: rgba(255,255,255,0.05); }
.telemetry-data {
  display: flex; justify-content: space-between; padding: 15px 0;
  font-family: var(--font-mono); font-size: 0.7rem; font-weight: 700;
  color: var(--text-muted); letter-spacing: 1px;
}

/* 动画系统 */
@keyframes pulse { 0% { opacity: 0.3; } 50% { opacity: 1; } 100% { opacity: 0.3; } }
@keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }
@keyframes slight-shake { 
  0% { transform: translate(1px, 1px) } 
  25% { transform: translate(-1px, -1px) } 
  50% { transform: translate(-1px, 1px) } 
  75% { transform: translate(1px, -1px) } 
  100% { transform: translate(1px, 1px) } 
}

/* 响应式降级 */
@media (max-width: 900px) {
  .manifest-core { flex-direction: column; gap: 40px; }
  .doom-header { flex-direction: column; align-items: flex-start; gap: 20px; }
  .timer-block { text-align: left; }
  .readout-value { font-size: 6rem; }
  .telemetry-data { flex-direction: column; gap: 10px; }
}
</style>