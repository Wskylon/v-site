<template>
  <div id="nexus-terminal" :class="[realmState, { 'death-return-active': isDeathReturning }]">
    
    <svg style="display: none;">
      <defs>
        <filter id="flesh-warp" x="-20%" y="-20%" width="140%" height="140%">
          <feTurbulence type="fractalNoise" baseFrequency="0.01 0.02" numOctaves="3" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="15" xChannelSelector="R" yChannelSelector="G" />
        </filter>
        <filter id="sanity-glitch">
          <feColorMatrix type="matrix" values="1 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 1 0" result="red"/>
          <feColorMatrix type="matrix" values="0 0 0 0 0  0 1 0 0 0  0 0 0 0 0  0 0 0 1 0" result="green"/>
          <feOffset in="red" dx="3" dy="0" result="red_offset"/>
          <feOffset in="green" dx="-3" dy="0" result="green_offset"/>
          <feBlend mode="screen" in="red_offset" in2="green_offset"/>
          <feBlend mode="screen" in="SourceGraphic" in2="red_offset"/>
        </filter>
      </defs>
    </svg>

    <div class="static-noise"></div>
    <div class="crt-scanlines"></div>

    <div class="grimoire-grid">

      <header class="grid-header">
        <div class="title-construct">
          <h1 class="eldritch-title" :data-text="terminalName">{{ terminalName }}</h1>
          <div class="sub-designation">
            <span class="blinking-eye">◉</span>
            <span class="designation-text">PROJECT_22408_USTC_ASCENSION // ALIVE</span>
          </div>
        </div>
        <div class="chronos-construct">
          <div class="doom-timer">{{ doomTimer }}</div>
          <div class="timer-label">UNTIL_THE_STARS_ARE_RIGHT (2028-12)</div>
        </div>
      </header>

      <aside class="grid-left-pillar">
        <h2 class="section-title">THE_PILLARS</h2>
        <div class="pillar-list">
          <div class="pillar-node" :class="{'active': activePillar === 'C_LANG'}">
            <span class="node-icon">Ⅰ</span>
            <div class="node-data">
              <div class="node-name">FLESH (C_LANGUAGE)</div>
              <div class="node-status">ASSIMILATION_ONGOING</div>
            </div>
          </div>
          <div class="pillar-node" :class="{'active': activePillar === 'DATA_STRUCT'}">
            <span class="node-icon">Ⅱ</span>
            <div class="node-data">
              <div class="node-name">BONE (DATA_STRUCTURES)</div>
              <div class="node-status">RECONSTRUCTING</div>
            </div>
          </div>
          <div class="pillar-node" :class="{'active': activePillar === 'MATH'}">
            <span class="node-icon">Ⅲ</span>
            <div class="node-data">
              <div class="node-name">VOID (ADVANCED_MATH)</div>
              <div class="node-status">COMPREHENDING</div>
            </div>
          </div>
        </div>

        <div class="flesh-forge-stats mt-auto">
          <h2 class="section-title">FLESH_FORGE (GYM)</h2>
          <div class="forge-bar-container">
            <div class="forge-label">CYCLE: 3-ON / 2-OFF</div>
            <div class="forge-bar"><div class="forge-progress" style="width: 60%"></div></div>
          </div>
          <div class="forge-bar-container mt-2">
            <div class="forge-label">CAFFEINE_CATALYST</div>
            <div class="forge-value text-crimson">{{ caffeineDose }} MG</div>
          </div>
        </div>
      </aside>

      <main class="grid-center-rift">
        <div class="rift-containment">
          <canvas ref="riftCanvas" class="rift-canvas"></canvas>
          <div class="rift-overlay"></div>
          
          <div class="sanity-centerpiece">
            <div class="vocab-readout">
              <span class="vocab-number">{{ vocabProgress }}</span>
              <span class="vocab-total">/ 4000</span>
            </div>
            <div class="vocab-label">GRIMOIRE_TRANSLATION_PROGRESS</div>
          </div>
        </div>
      </main>

      <aside class="grid-right-log">
        <h2 class="section-title">SANITY_METRICS</h2>
        <div class="metric-block">
          <div class="metric-row">
            <span>KARMA_LOAD (业障):</span>
            <span class="text-glitch">{{ karmaLoad.toFixed(2) }}</span>
          </div>
          <div class="metric-row">
            <span>DEATH_RETURNS (死亡回归):</span>
            <span class="text-crimson">[{{ deathReturns }}]</span>
          </div>
          <div class="metric-row">
            <span>SENIOR_SISTER_GAZE:</span>
            <span :class="{'text-crimson blink': isGazed}">{{ isGazed ? 'DETECTED' : 'ABSENT' }}</span>
          </div>
        </div>

        <h2 class="section-title mt-4">AKASHIC_RECORDS</h2>
        <div class="terminal-history" ref="historyBox">
          <div v-for="(log, idx) in cmdHistory" :key="idx" class="log-entry" :class="log.type">
            <span class="log-time">[{{ log.time }}]</span>
            <span class="log-msg" v-html="log.msg"></span>
          </div>
        </div>
      </aside>

      <footer class="grid-footer">
        <div class="cli-prefix">root@vzuor:~#</div>
        <input 
          ref="cmdInput"
          v-model="currentCmd"
          @keyup.enter="executeCommand"
          class="cli-input"
          placeholder="AWAITING SACRIFICE... (type 'help' for rituals)"
          spellcheck="false"
          autocomplete="off"
        />
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick } from 'vue'

// ==========================================
// [系统核心架构] : 状态与持久化
// ==========================================
const terminalName = "V'ZUOR KHAA-SH'AN"
const realmState = ref('realm-stable') // stable, hyper, corrupted
const isDeathReturning = ref(false)

// 响应式数据 (与本地 LocalStorage 绑定，确保重启不丢失)
const loadState = (key, defaultVal) => Number(localStorage.getItem(key)) || defaultVal
const saveState = (key, val) => localStorage.setItem(key, val)

const vocabProgress = ref(loadState('vzuor_vocab', 2000))
const karmaLoad = ref(loadState('vzuor_karma', 13.4))
const deathReturns = ref(loadState('vzuor_deaths', 0))
const caffeineDose = ref(200)

const activePillar = ref('C_LANG')
const isGazed = ref(false)

const doomTimer = ref('000D | 00:00:00')

// 终端日志历史
const cmdHistory = reactive([
  { time: getSysTime(), type: 'sys', msg: '初始化阿卡姆底层协议... 成功。' },
  { time: getSysTime(), type: 'sys', msg: '22408 飞升矩阵已上线。' }
])
const currentCmd = ref('')
const historyBox = ref(null)

// ==========================================
// [时间引擎] : 倒计时与时钟
// ==========================================
function getSysTime() {
  const now = new Date()
  return `${now.getHours().toString().padStart(2,'0')}:${now.getMinutes().toString().padStart(2,'0')}:${now.getSeconds().toString().padStart(2,'0')}`
}

let timerInterval
const initTimeEngine = () => {
  const targetDate = new Date('2028-12-23T08:30:00').getTime()
  timerInterval = setInterval(() => {
    const now = new Date().getTime()
    const diff = targetDate - now
    if (diff <= 0) {
      doomTimer.value = "JUDGEMENT_DAY"
      return
    }
    const d = Math.floor(diff / 86400000).toString().padStart(3, '0')
    const h = Math.floor((diff / 3600000) % 24).toString().padStart(2, '0')
    const m = Math.floor((diff / 60000) % 60).toString().padStart(2, '0')
    const s = Math.floor((diff / 1000) % 60).toString().padStart(2, '0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
  }, 1000)
}

// ==========================================
// [指令解析中枢] : 复杂指令处理系统
// ==========================================
const pushLog = (msg, type = 'info') => {
  cmdHistory.push({ time: getSysTime(), type, msg })
  if (cmdHistory.length > 50) cmdHistory.shift()
  nextTick(() => {
    if (historyBox.value) historyBox.value.scrollTop = historyBox.value.scrollHeight
  })
}

const executeCommand = () => {
  const rawCmd = currentCmd.value.trim()
  if (!rawCmd) return
  
  pushLog(`> ${rawCmd}`, 'echo')
  const args = rawCmd.toLowerCase().split(' ')
  const command = args[0]

  switch (command) {
    case 'help':
      pushLog('可用仪式 [ rituals ]:', 'sys')
      pushLog('  vocab [num]   - 献祭词汇 (例: vocab 50)', 'sys')
      pushLog('  dose [num]    - 注入咖啡因 (例: dose 400)', 'sys')
      pushLog('  pillar [name] - 切换研究柱神 (c/data/math)', 'sys')
      pushLog('  vzuor         - [禁忌] 呼唤真名', 'sys')
      pushLog('  die           - 执行死亡回归协议', 'sys')
      break

    case 'vocab':
      const amount = parseInt(args[1]) || 1
      vocabProgress.value = Math.min(4000, vocabProgress.value + amount)
      saveState('vzuor_vocab', vocabProgress.value)
      karmaLoad.value = Math.max(0, karmaLoad.value - (amount * 0.05))
      saveState('vzuor_karma', karmaLoad.value)
      pushLog(`魔典翻译进度更新。当前词汇量: <span class="text-bone">${vocabProgress.value}/4000</span>`, 'success')
      break

    case 'dose':
      const dose = parseInt(args[1])
      if (isNaN(dose)) {
        pushLog('无效的剂量。', 'error')
      } else {
        caffeineDose.value = dose
        if (dose >= 400) {
          triggerHyperDrive()
        } else {
          pushLog(`咖啡因水平已稳定在 ${dose}mg。`, 'info')
        }
      }
      break

    case 'pillar':
      const p = args[1]
      if (p === 'c') activePillar.value = 'C_LANG'
      else if (p === 'data') activePillar.value = 'DATA_STRUCT'
      else if (p === 'math') activePillar.value = 'MATH'
      else pushLog('未知的柱神。', 'error')
      if (['c','data','math'].includes(p)) pushLog(`认知焦点已转移至 [${activePillar.value}]。`, 'success')
      break

    case 'vzuor':
      triggerSanityLoss()
      break

    case 'die':
      triggerDeathReturn()
      break

    default:
      karmaLoad.value += 0.2
      saveState('vzuor_karma', karmaLoad.value)
      pushLog(`未知指令。深渊注视着你，业障增加。`, 'error')
  }
  
  currentCmd.value = ''
}

// --- 极端状态触发器 ---
const triggerHyperDrive = () => {
  realmState.value = 'realm-hyper'
  pushLog('警告：咖啡因摄入过载！神经元燃烧中...', 'error')
  setTimeout(() => {
    realmState.value = 'realm-stable'
    caffeineDose.value = 200
    pushLog('系统冷却，药效消退，恢复常规 3-ON-2-OFF 循环。', 'sys')
  }, 8000)
}

const triggerSanityLoss = () => {
  realmState.value = 'realm-corrupted'
  isGazed.value = true
  pushLog('<span class="text-glitch">“学姐在看着你。”</span>', 'error')
  setTimeout(() => {
    realmState.value = 'realm-stable'
    isGazed.value = false
    pushLog('幻觉消散。请保持理智。', 'sys')
  }, 5000)
}

const triggerDeathReturn = () => {
  isDeathReturning.value = true
  deathReturns.value += 1
  saveState('vzuor_deaths', deathReturns.value)
  pushLog('<span class="text-crimson">检测到致命逻辑错误。正在执行死亡回归...</span>', 'error')
  setTimeout(() => {
    isDeathReturning.value = false
    pushLog(`世界线重置。这是你的第 ${deathReturns.value} 次尝试。`, 'sys')
  }, 3000)
}


// ==========================================
// [非欧几里得渲染引擎] : 高级 Canvas 粒子与触手
// ==========================================
const riftCanvas = ref(null)
let ctx, animationId
let particles = []
let time = 0

class AbyssalParticle {
  constructor(w, h) {
    this.w = w; this.h = h
    this.reset()
  }
  reset() {
    this.x = this.w / 2 + (Math.random() - 0.5) * 50
    this.y = this.h / 2 + (Math.random() - 0.5) * 50
    this.angle = Math.random() * Math.PI * 2
    this.speed = Math.random() * 0.5 + 0.1
    this.radius = Math.random() * 2 + 1
    this.life = Math.random() * 100 + 50
    this.maxLife = this.life
    this.color = realmState.value === 'realm-hyper' ? `rgba(139,0,0,` : `rgba(0, 255, 102,` // 红 or 虚空绿
  }
  update(progressRatio) {
    // 粒子受到中央黑洞（理智值）的吸引或排斥
    const centerX = this.w / 2
    const centerY = this.h / 2
    const dx = centerX - this.x
    const dy = centerY - this.y
    const dist = Math.sqrt(dx*dx + dy*dy)
    
    // 复杂的极坐标运动
    this.angle += 0.02
    this.x += Math.cos(this.angle) * this.speed + (dx / dist) * (progressRatio * 2)
    this.y += Math.sin(this.angle) * this.speed + (dy / dist) * (progressRatio * 2)
    
    this.life--
    if (this.life <= 0 || dist > this.w) this.reset()
  }
  draw(ctx) {
    const opacity = (this.life / this.maxLife) * 0.6
    ctx.fillStyle = `${this.color}${opacity})`
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2)
    ctx.fill()
  }
}

const initRiftEngine = () => {
  if (!riftCanvas.value) return
  ctx = riftCanvas.value.getContext('2d')
  
  // 匹配 CSS 中的容器尺寸
  const width = riftCanvas.value.clientWidth || 400
  const height = riftCanvas.value.clientHeight || 400
  riftCanvas.value.width = width
  riftCanvas.value.height = height

  // 初始化粒子群
  for (let i = 0; i < 150; i++) {
    particles.push(new AbyssalParticle(width, height))
  }

  const renderLoop = () => {
    // 制造拖影效果
    ctx.fillStyle = 'rgba(5, 5, 5, 0.15)'
    ctx.fillRect(0, 0, width, height)

    const progressRatio = vocabProgress.value / 4000
    
    // 绘制深渊底部的蠕动触手/波浪
    ctx.beginPath()
    const waveHeight = height * (1 - progressRatio * 0.8) // 随词汇量上升，深渊被填满
    ctx.moveTo(0, waveHeight)
    for (let x = 0; x <= width; x++) {
      const isH = realmState.value === 'realm-hyper'
      const freq = isH ? 0.05 : 0.02
      const amp = isH ? 30 : 15
      const y = waveHeight + Math.sin(x * freq + time) * amp + Math.cos(x * 0.01 + time * 1.5) * (amp/2)
      ctx.lineTo(x, y)
    }
    ctx.lineTo(width, height)
    ctx.lineTo(0, height)
    
    const grad = ctx.createLinearGradient(0, waveHeight, 0, height)
    grad.addColorStop(0, realmState.value === 'realm-hyper' ? '#4a0000' : '#0a1a15')
    grad.addColorStop(1, '#000000')
    ctx.fillStyle = grad
    ctx.fill()

    // 更新和绘制粒子
    particles.forEach(p => {
      // 狂化模式下动态改变颜色
      p.color = realmState.value === 'realm-hyper' ? `rgba(200,20,20,` : `rgba(20, 200, 100,`
      p.update(progressRatio)
      p.draw(ctx)
    })

    // 绘制中心“瞳孔/核心”
    const coreRadius = 30 + Math.sin(time * 2) * 5
    ctx.beginPath()
    ctx.arc(width/2, height/2, coreRadius, 0, Math.PI * 2)
    ctx.strokeStyle = realmState.value === 'realm-hyper' ? 'rgba(255,0,0,0.5)' : 'rgba(0,255,102,0.3)'
    ctx.lineWidth = 2
    ctx.stroke()

    time += realmState.value === 'realm-hyper' ? 0.1 : 0.03
    animationId = requestAnimationFrame(renderLoop)
  }
  renderLoop()
}

// --- 生命周期 ---
onMounted(() => {
  initTimeEngine()
  // 延迟初始化 Canvas 确保 DOM 就绪
  setTimeout(initRiftEngine, 100)
  pushLog('终端所有核心系统已就绪。', 'success')
})

onBeforeUnmount(() => {
  clearInterval(timerInterval)
  cancelAnimationFrame(animationId)
})

</script>

<style>
/* ==========================================
   [深渊美学与绝对网格布局系统]
   ========================================== */

@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Fira+Code:wght@400;700&display=swap');

:root {
  /* 调色板: 虚空黑, 骨灰白, 终端绿, 祭祀红 */
  --c-void: #050505;
  --c-bone: #E2DED0;
  --c-terminal: #00FF66;
  --c-terminal-dim: #005522;
  --c-crimson: #8B0000;
  --c-crimson-bright: #FF1A1A;
  --c-muted: #4A4A4A;
  
  --f-title: 'Cinzel', serif;
  --f-code: 'Fira Code', monospace;
}

body, html {
  margin: 0; padding: 0; width: 100%; height: 100%;
  background-color: var(--c-void); color: var(--c-bone);
  font-family: var(--f-code); overflow: hidden;
  -webkit-font-smoothing: antialiased;
}

/* --- 全局特效 --- */
.static-noise {
  position: fixed; inset: 0; z-index: 1; pointer-events: none;
  background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="3" stitchTiles="stitch"/></filter><rect width="200" height="200" filter="url(%23n)"/></svg>');
  opacity: 0.05;
}
.crt-scanlines {
  position: fixed; inset: 0; z-index: 2; pointer-events: none;
  background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
  background-size: 100% 4px, 6px 100%;
}

.text-crimson { color: var(--c-crimson-bright); }
.text-bone { color: var(--c-bone); font-weight: bold; }
.mt-auto { margin-top: auto; }
.mt-2 { margin-top: 10px; }
.mt-4 { margin-top: 20px; }

/* 死亡回归全屏覆盖 */
.death-return-active::after {
  content: ''; position: fixed; inset: 0; z-index: 9999;
  background: black; animation: death-flash 3s cubic-bezier(0.19, 1, 0.22, 1) forwards; pointer-events: none;
}
@keyframes death-flash { 0% { opacity: 1; background: var(--c-crimson-bright); } 10% { background: black; } 100% { opacity: 0; } }

/* --- 核心网格布局 (CSS Grid) --- */
.grimoire-grid {
  position: relative; z-index: 10;
  width: 100vw; height: 100vh; padding: 2rem; box-sizing: border-box;
  display: grid;
  /* 严格的 3 列 3 行布局，彻底杜绝重叠 */
  grid-template-columns: 320px 1fr 350px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header header"
    "left center right"
    "footer footer footer";
  gap: 2rem;
}

/* ==========================================
   [顶部：Header]
   ========================================== */
.grid-header {
  grid-area: header;
  display: flex; justify-content: space-between; align-items: flex-end;
  border-bottom: 2px solid var(--c-muted); padding-bottom: 1rem;
}
.title-construct { display: flex; flex-direction: column; }
.eldritch-title {
  font-family: var(--f-title); font-size: clamp(2rem, 3vw, 3.5rem);
  font-weight: 900; margin: 0; letter-spacing: 0.1em;
  text-shadow: 0 0 10px rgba(226, 222, 208, 0.2);
}
.sub-designation { display: flex; align-items: center; gap: 0.5rem; margin-top: 0.5rem; font-size: 0.75rem; color: var(--c-terminal-dim); }
.blinking-eye { color: var(--c-crimson-bright); animation: blink 2s infinite; }

.chronos-construct { text-align: right; }
.doom-timer { font-size: clamp(1.5rem, 2.5vw, 2.8rem); font-weight: 700; color: var(--c-bone); letter-spacing: -1px; }
.timer-label { font-size: 0.65rem; color: var(--c-muted); letter-spacing: 2px; margin-top: 0.5rem; }

/* ==========================================
   [左翼：Pillars & Forge]
   ========================================== */
.grid-left-pillar {
  grid-area: left; display: flex; flex-direction: column;
  border-right: 1px dashed var(--c-muted); padding-right: 2rem;
}
.section-title { font-size: 0.85rem; color: var(--c-muted); letter-spacing: 3px; border-bottom: 1px solid var(--c-muted); padding-bottom: 0.5rem; margin-bottom: 1.5rem; text-transform: uppercase; }

.pillar-list { display: flex; flex-direction: column; gap: 1.5rem; }
.pillar-node { display: flex; align-items: center; gap: 1rem; opacity: 0.4; transition: opacity 0.3s; }
.pillar-node.active { opacity: 1; }
.pillar-node.active .node-icon { color: var(--c-terminal); text-shadow: 0 0 10px var(--c-terminal-dim); border-color: var(--c-terminal); }
.pillar-node.active .node-status { color: var(--c-terminal); }
.node-icon { width: 40px; height: 40px; border: 1px solid var(--c-muted); display: flex; align-items: center; justify-content: center; font-family: var(--f-title); font-size: 1.2rem; }
.node-name { font-weight: 700; font-size: 0.9rem; }
.node-status { font-size: 0.7rem; color: var(--c-muted); margin-top: 0.2rem; }

.forge-bar-container { width: 100%; }
.forge-label { font-size: 0.75rem; margin-bottom: 0.5rem; color: var(--c-bone); }
.forge-bar { height: 6px; background: #111; border: 1px solid var(--c-muted); }
.forge-progress { height: 100%; background: var(--c-terminal); box-shadow: 0 0 10px var(--c-terminal-dim); }
.forge-value { font-size: 1.5rem; font-weight: 700; }

/* ==========================================
   [中枢：Rift Canvas]
   ========================================== */
.grid-center-rift {
  grid-area: center; display: flex; justify-content: center; align-items: center;
  position: relative;
}
.rift-containment {
  width: 100%; max-width: 450px; aspect-ratio: 3/4;
  position: relative; border: 1px solid var(--c-muted);
  background: #020202; overflow: hidden;
  /* 关键：应用 SVG 扭曲滤镜制造不可名状感 */
  filter: url(#flesh-warp);
  box-shadow: 0 0 50px rgba(0,0,0,0.8), inset 0 0 40px rgba(0,0,0,0.9);
  border-radius: 5px 50px 5px 50px; /* 异形裁剪 */
}
.rift-canvas { width: 100%; height: 100%; display: block; }
.rift-overlay { position: absolute; inset: 0; background: radial-gradient(circle at center, transparent 30%, rgba(0,0,0,0.8) 100%); pointer-events: none; }

.sanity-centerpiece {
  position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
  text-align: center; pointer-events: none; z-index: 5;
  background: rgba(5,5,5,0.7); padding: 1.5rem; border: 1px solid var(--c-muted);
  backdrop-filter: blur(4px); border-radius: 50%; aspect-ratio: 1; display: flex; flex-direction: column; justify-content: center;
}
.vocab-number { font-family: var(--f-title); font-size: 3rem; font-weight: 900; color: var(--c-bone); }
.vocab-total { font-size: 1rem; color: var(--c-muted); }
.vocab-label { font-size: 0.6rem; letter-spacing: 2px; margin-top: 0.5rem; color: var(--c-terminal-dim); }

/* ==========================================
   [右翼：Log & Sanity]
   ========================================== */
.grid-right-log {
  grid-area: right; display: flex; flex-direction: column;
  border-left: 1px dashed var(--c-muted); padding-left: 2rem;
}
.metric-block { display: flex; flex-direction: column; gap: 1rem; font-size: 0.85rem; }
.metric-row { display: flex; justify-content: space-between; border-bottom: 1px solid rgba(255,255,255,0.05); padding-bottom: 0.5rem; }

.terminal-history {
  flex: 1; overflow-y: auto; background: rgba(0,0,0,0.3); border: 1px solid var(--c-muted);
  padding: 1rem; display: flex; flex-direction: column; gap: 0.5rem;
  font-size: 0.75rem; line-height: 1.4; scrollbar-width: thin; scrollbar-color: var(--c-muted) transparent;
}
.log-entry { display: flex; gap: 0.5rem; word-break: break-all; }
.log-time { color: var(--c-muted); flex-shrink: 0; }
.log-entry.info .log-msg { color: var(--c-bone); }
.log-entry.sys .log-msg { color: var(--c-terminal-dim); }
.log-entry.echo .log-msg { color: #888; font-style: italic; }
.log-entry.success .log-msg { color: var(--c-terminal); }
.log-entry.error .log-msg { color: var(--c-crimson-bright); }

/* ==========================================
   [底部：CLI Footer]
   ========================================== */
.grid-footer {
  grid-area: footer; display: flex; align-items: center; gap: 1rem;
  border-top: 2px solid var(--c-muted); padding-top: 1rem;
}
.cli-prefix { color: var(--c-terminal); font-weight: 700; font-size: 1.2rem; }
.cli-input {
  flex: 1; background: transparent; border: none; outline: none;
  color: var(--c-bone); font-family: var(--f-code); font-size: 1.2rem;
}
.cli-input::placeholder { color: var(--c-muted); font-style: italic; }

/* ==========================================
   [状态变形：狂化与理智丧失]
   ========================================== */
.realm-hyper {
  --c-void: #1a0000; --c-bone: #ffcccc; --c-terminal: #ff1a1a; --c-terminal-dim: #8b0000;
}
.realm-hyper .grimoire-grid { animation: shake 0.5s infinite; }
.realm-hyper .rift-containment { box-shadow: 0 0 100px rgba(255,0,0,0.5); border-color: var(--c-crimson-bright); filter: url(#flesh-warp) drop-shadow(0 0 20px red); }

.realm-corrupted .eldritch-title { animation: text-glitch 0.2s infinite; filter: url(#sanity-glitch); }
.realm-corrupted .void-background { background: white; filter: invert(1); }
.realm-corrupted .static-noise { opacity: 0.2; }

/* ==========================================
   [动画 Keyframes]
   ========================================== */
@keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }
@keyframes shake { 0% { transform: translate(1px,1px); } 25% { transform: translate(-1px,-1px); } 50% { transform: translate(-1px,1px); } 75% { transform: translate(1px,-1px); } 100% { transform: translate(1px,1px); } }
@keyframes text-glitch { 0% { transform: skewX(2deg); } 50% { transform: skewX(-2deg); } 100% { transform: skewX(0); } }

/* ==========================================
   [响应式降级防御]
   ========================================== */
@media (max-width: 1200px) {
  .grimoire-grid {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto 500px auto auto;
    grid-template-areas: "header" "center" "left" "right" "footer";
    overflow-y: auto; height: auto;
  }
  .grid-left-pillar, .grid-right-log { border: none; padding: 0; border-top: 1px dashed var(--c-muted); padding-top: 2rem; }
  .rift-containment { max-width: 100%; }
}
</style>