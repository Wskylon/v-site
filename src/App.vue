<template>
  <div id="vzuor-nexus" class="nexus-root" :class="realmState">
    
    <div class="layer-0-parchment" :style="{ backgroundImage: `url(${require('./assets/羊皮纸.jpg')})` }"></div>
    <div class="micro-noise-overlay"></div>

    <div class="layer-1-corruption">
      <img src="./assets/墨迹1.jpg" class="ink-asset ink-top-left" />
      <img src="./assets/墨迹2.jpg" class="ink-asset ink-bottom-right" />
      <img src="./assets/触手1.jpg" class="ink-asset sketch-bg-left" />
      <img src="./assets/墨迹3.jpg" class="ink-asset hand-of-void" />
    </div>

    <canvas ref="engineCanvas" class="layer-2-canvas"></canvas>

    <div class="layer-3-altar">
      <div class="altar-container" :class="{'heartbeat': isHyper}">
        <img src="./assets/耶稣受难克苏鲁版，作为祭坛.jpg" class="crucifixion-totem" />
        
        <div class="siphon-core">
          <div class="core-ring">
             <span class="vocab-digit">{{ vocabSiphoned }}</span>
             <span class="vocab-max">/ 4000</span>
          </div>
          <div class="core-label">GRIMOIRE_ASSIMILATION</div>
        </div>

        <img src="./assets/墨迹4.jpg" class="ink-asset blood-splash-left" :class="{'active': isHyper}" />
        <img src="./assets/墨迹5.jpg" class="ink-asset blood-splash-right" :class="{'active': isHyper}" />
      </div>
    </div>

    <div class="layer-4-order-grid">
      
      <header class="grid-cell header-zone">
        <div class="brand-block">
          <h1 class="artifact-title">V'ZUOR KHAA-SH'AN</h1>
          <div class="artifact-sub">
            <span class="pulse-node"></span>
            OBSERVER_HUB // 22408_ASCENSION_PROTOCOL
          </div>
        </div>
        <div class="time-block">
          <div class="doom-clock">{{ doomTime }}</div>
          <div class="doom-label">DESTINY_RECKONING (2028)</div>
        </div>
      </header>

      <aside class="grid-cell left-zone">
        <h2 class="zone-title">BIOMETRIC_TELEMETRY</h2>
        <div class="telemetry-matrix">
          <div class="t-row">
            <span class="t-key">SANITY_INDEX:</span>
            <span class="t-val text-alert">{{ sanityIndex.toFixed(4) }}%</span>
          </div>
          <div class="t-row">
            <span class="t-key">KARMA_WEIGHT:</span>
            <span class="t-val">{{ karmaWeight.toFixed(2) }}</span>
          </div>
          <div class="t-row">
            <span class="t-key">FLESH_FORGE:</span>
            <span class="t-val">3-ON / 2-OFF</span>
          </div>
          <div class="t-row">
            <span class="t-key">CAFFEINE_DOSE:</span>
            <span class="t-val" :class="{'text-hyper': isHyper}">{{ caffeineLvl }} MG</span>
          </div>
        </div>
        
        <div class="asset-showcase">
           <img src="./assets/触手2.jpg" class="ink-asset mini-sketch" />
           <p class="sketch-caption">FIG 1. THE_ABYSSAL_ROOTS</p>
        </div>
      </aside>

      <aside class="grid-cell right-zone">
        <h2 class="zone-title">AKASHIC_RECORDS</h2>
        <div class="record-scroll" ref="scrollBox">
          <div v-for="(log, i) in terminalLogs" :key="i" class="log-line" :class="log.level">
            <span class="log-ts">[{{ log.ts }}]</span>
            <span class="log-msg">{{ log.msg }}</span>
          </div>
        </div>
      </aside>

      <footer class="grid-cell footer-zone">
        <div class="terminal-input-wrap">
          <span class="cli-arrow">>></span>
          <input 
            ref="cmdInput"
            v-model="rawCommand"
            @keyup.enter="processRitual"
            class="cli-input"
            placeholder="AWAITING SACRIFICE... (e.g., 'vocab 50', 'dose 400', 'vzuor')"
            spellcheck="false"
            autocomplete="off"
          />
        </div>
      </footer>

    </div>
    
    <div class="sanity-break-overlay" v-if="isSanityBroken">
       <h1 class="glitch-huge">H I D E Y O U R S E L F</h1>
    </div>

  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick } from 'vue'

// ============================================================================
// [系统底层：持久化与核心状态]
// ============================================================================
const loadMem = (k, def) => Number(localStorage.getItem(k)) || def
const saveMem = (k, v) => localStorage.setItem(k, v)

// 响应式核心数据
const vocabSiphoned = ref(loadMem('vzuor_vocab_v4', 2150))
const karmaWeight = ref(loadMem('vzuor_karma_v4', 23.7))
const caffeineLvl = ref(200)
const sanityIndex = ref(99.9999)

// 视图状态
const realmState = ref('state-stable')
const isHyper = ref(false)
const isSanityBroken = ref(false)
const rawCommand = ref('')
const terminalLogs = reactive([])
const scrollBox = ref(null)
const doomTime = ref('000D | 00:00:00')

// ============================================================================
// [时间引擎：倒计时与时钟同步]
// ============================================================================
const getTs = () => {
  const d = new Date()
  return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}.${d.getMilliseconds().toString().padStart(3,'0')}`
}

let timeLoop
const bootTimeEngine = () => {
  const targetDate = new Date('2028-12-23T08:30:00').getTime()
  timeLoop = setInterval(() => {
    const diff = targetDate - new Date().getTime()
    if (diff <= 0) { doomTime.value = "THE_END_IS_HERE"; return }
    const d = Math.floor(diff / 86400000).toString().padStart(3, '0')
    const h = Math.floor((diff / 3600000) % 24).toString().padStart(2, '0')
    const m = Math.floor((diff / 60000) % 60).toString().padStart(2, '0')
    const s = Math.floor((diff / 1000) % 60).toString().padStart(2, '0')
    doomTime.value = `${d}D | ${h}:${m}:${s}`
    
    // 理智值随时间缓慢流失
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001)
  }, 1000)
}

// ============================================================================
// [命令行中枢：仪式解析器]
// ============================================================================
const appendLog = (msg, level = 'info') => {
  terminalLogs.push({ ts: getTs(), msg, level })
  if (terminalLogs.length > 60) terminalLogs.shift()
  nextTick(() => { if (scrollBox.value) scrollBox.value.scrollTop = scrollBox.value.scrollHeight })
}

const processRitual = () => {
  const cmdStr = rawCommand.value.trim().toLowerCase()
  if (!cmdStr) return
  
  appendLog(`> ${cmdStr}`, 'echo')
  const args = cmdStr.split(' ')
  const rootCmd = args[0]

  switch (rootCmd) {
    case 'vocab':
      const amt = parseInt(args[1])
      if (isNaN(amt)) {
        appendLog('献祭失败：未定义数量。', 'error')
      } else {
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
        saveMem('vzuor_vocab_v4', vocabSiphoned.value)
        karmaWeight.value = Math.max(0, karmaWeight.value - (amt * 0.05))
        saveMem('vzuor_karma_v4', karmaWeight.value)
        sanityIndex.value = Math.min(100, sanityIndex.value + 0.5)
        appendLog(`知识已同化。祭坛水位上升。当前: ${vocabSiphoned.value}/4000`, 'success')
      }
      break

    case 'dose':
      const dose = parseInt(args[1])
      if (!isNaN(dose)) {
        caffeineLvl.value = dose
        if (dose >= 400) triggerHyperMode()
        else appendLog(`神经递质催化剂已调整为 ${dose}MG。`, 'info')
      }
      break

    case 'vzuor':
      triggerSanityBreak()
      break

    case 'help':
      appendLog('仪式协议 (PROTOCOLS):', 'info')
      appendLog(' - vocab [num] : 献祭考研词汇进度', 'info')
      appendLog(' - dose [num]  : 注入咖啡因 (MG)', 'info')
      appendLog(' - vzuor       : [WARNING] 呼唤真名', 'alert')
      break

    default:
      karmaWeight.value += 0.5
      saveMem('vzuor_karma_v4', karmaWeight.value)
      appendLog(`未知频率 [${cmdStr}]。业障增加。古神在注视你。`, 'error')
  }
  rawCommand.value = ''
}

// ============================================================================
// [极端状态触发器]
// ============================================================================
const triggerHyperMode = () => {
  isHyper.value = true
  realmState.value = 'state-hyper'
  appendLog('CRITICAL: 心率异常，突触燃烧。血液开始沸腾。', 'alert')
  // 通知 Canvas 引擎加速
  if(window.sysEngine) window.sysEngine.setHyper(true)
  
  setTimeout(() => {
    isHyper.value = false
    realmState.value = 'state-stable'
    caffeineLvl.value = 200
    if(window.sysEngine) window.sysEngine.setHyper(false)
    appendLog('药效消退。机体恢复 3-ON-2-OFF 循环。', 'info')
  }, 8000)
}

const triggerSanityBreak = () => {
  isSanityBroken.value = true
  appendLog('FATAL ERROR: 实体突破防线。', 'alert')
  setTimeout(() => {
    isSanityBroken.value = false
    sanityIndex.value -= 15.0
    appendLog('世界线重置完成。理智大幅受损。', 'error')
  }, 4000)
}

// ============================================================================
// [终极渲染：基于反向运动学(IK)与流体粒子的 Canvas 引擎]
// 代码深度突破：手写物理模拟，直接操控屏幕像素
// ============================================================================
const engineCanvas = ref(null)

class IKTentacle {
  constructor(x, y, segments, length, color) {
    this.baseX = x; this.baseY = y
    this.segments = segments; this.segLength = length
    this.angles = new Array(segments).fill(0)
    this.target = { x: x, y: y }
    this.color = color
    this.noiseOffset = Math.random() * 1000
  }
  
  reach(targetX, targetY) {
    // 逆运动学简易实现 (FABRIK 变体)
    let curX = targetX, curY = targetY
    const pts = [{x: this.baseX, y: this.baseY}]
    for(let i=0; i<this.segments; i++) pts.push({x:0, y:0}) // init
    
    // 简易追踪算法 (向目标弯曲)
    for (let i = 0; i < this.segments; i++) {
      let dx = targetX - this.baseX
      let dy = targetY - this.baseY
      let angle = Math.atan2(dy, dx) + Math.sin(Date.now()*0.001 + i*0.2 + this.noiseOffset) * 0.3
      this.angles[i] += (angle - this.angles[i]) * 0.1
    }
  }

  draw(ctx, time, isHyper) {
    ctx.beginPath()
    ctx.moveTo(this.baseX, this.baseY)
    let cx = this.baseX, cy = this.baseY
    
    // 触手根部粗，尖端细
    for (let i = 0; i < this.segments; i++) {
      // 增加蠕动感
      const wobble = Math.sin(time * (isHyper?5:2) + i * 0.5 + this.noiseOffset) * (isHyper?15:5)
      cx += Math.cos(this.angles[i]) * this.segLength + wobble * 0.1
      cy += Math.sin(this.angles[i]) * this.segLength + wobble * 0.1
      
      ctx.lineTo(cx, cy)
    }
    
    ctx.strokeStyle = this.color
    ctx.lineWidth = isHyper ? 3 : 1.5
    ctx.lineCap = 'round'
    ctx.stroke()
  }
}

class BloodParticle {
  constructor(w, h) {
    this.w = w; this.h = h
    this.reset()
  }
  reset() {
    this.x = this.w / 2 + (Math.random() - 0.5) * 400
    this.y = this.h / 2 + (Math.random() - 0.5) * 400
    this.vx = (Math.random() - 0.5) * 2
    this.vy = Math.random() * -2 - 1 // 向上漂浮，像灰烬
    this.life = Math.random() * 100 + 50
    this.maxLife = this.life
    this.size = Math.random() * 2.5 + 0.5
  }
  update(isHyper) {
    this.x += this.vx * (isHyper ? 4 : 1)
    this.y += this.vy * (isHyper ? 4 : 1)
    
    // 引力：如果靠近中心，会被祭坛吸进去
    const dx = (this.w/2) - this.x
    const dy = (this.h/2) - this.y
    if(Math.sqrt(dx*dx+dy*dy) < 150) {
        this.vx += dx * 0.005
        this.vy += dy * 0.005
    }

    this.life--
    if (this.life <= 0 || this.y < 0 || this.x < 0 || this.x > this.w) this.reset()
  }
  draw(ctx, isHyper) {
    const op = (this.life / this.maxLife) * 0.8
    ctx.fillStyle = isHyper ? `rgba(180, 0, 0, ${op})` : `rgba(20, 20, 20, ${op})`
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2)
    ctx.fill()
  }
}

const bootRenderEngine = () => {
  const cvs = engineCanvas.value
  if(!cvs) return
  const ctx = cvs.getContext('2d')
  
  let w = window.innerWidth
  let h = window.innerHeight
  cvs.width = w; cvs.height = h

  // 生成围绕祭坛的动态触手
  const tentacles = []
  for(let i=0; i<8; i++) {
    tentacles.push(new IKTentacle(w/2, h/2, 15, 20, 'rgba(10, 10, 10, 0.4)'))
  }

  // 生成环境灰烬/血滴
  const particles = []
  for(let i=0; i<150; i++) particles.push(new BloodParticle(w, h))

  let mx = w/2, my = h/2
  window.addEventListener('mousemove', (e) => { mx = e.clientX; my = e.clientY })
  window.addEventListener('resize', () => { 
    w = window.innerWidth; h = window.innerHeight; 
    cvs.width = w; cvs.height = h 
    tentacles.forEach(t => { t.baseX = w/2; t.baseY = h/2 })
  })

  let time = 0
  let isEngineHyper = false
  
  // 暴露给 Vue 组件控制
  window.sysEngine = { setHyper: (val) => isEngineHyper = val }

  const loop = () => {
    ctx.clearRect(0, 0, w, h)

    // 更新绘制触手
    tentacles.forEach((t, index) => {
      // 让部分触手具有独立意志，部分追踪鼠标
      const targetX = index % 2 === 0 ? mx : w/2 + Math.cos(time + index)*300
      const targetY = index % 2 === 0 ? my : h/2 + Math.sin(time*0.8 + index)*300
      t.reach(targetX, targetY)
      t.color = isEngineHyper ? 'rgba(139,0,0,0.6)' : 'rgba(10,10,10,0.3)'
      t.draw(ctx, time, isEngineHyper)
    })

    // 更新绘制粒子
    particles.forEach(p => {
      p.update(isEngineHyper)
      p.draw(ctx, isEngineHyper)
    })

    time += isEngineHyper ? 0.05 : 0.01
    requestAnimationFrame(loop)
  }
  loop()
}

// ============================================================================
// [生命周期钩子]
// ============================================================================
onMounted(() => {
  appendLog('BOOT SEQUENCE INITIATED...', 'sys')
  appendLog('加载羊皮纸纹理底层...', 'sys')
  appendLog('应用 Multiply 正片叠底光学遮罩...', 'sys')
  appendLog('唤醒非欧几里得 Canvas 渲染管线...', 'sys')
  
  bootTimeEngine()
  setTimeout(bootRenderEngine, 500) // 延迟确保 DOM 尺寸就绪
  
  appendLog('V\'ZUOR 矩阵连接成功。神识已固定。', 'success')
})

onBeforeUnmount(() => {
  clearInterval(timeLoop)
})
</script>

<style>
/* ============================================================================
   [CSS 核心架构：融合、叠底与绝对隔离]
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=IM+Fell+English+SC&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  --c-text-main: #1a1515; /* 极深的棕黑，像干涸的墨水 */
  --c-text-muted: #5c554b;
  --c-blood: #8B0000;
  --c-blood-bright: #cc0000;
  
  --f-title: 'Cinzel', serif;
  --f-text: 'IM Fell English SC', serif;
  --f-mono: 'Space Mono', monospace;
}

html, body {
  margin: 0; padding: 0; width: 100vw; height: 100vh;
  background-color: #d1c8b4; /* Fallback color matching parchment */
  color: var(--c-text-main); font-family: var(--f-mono);
  overflow: hidden;
}

.nexus-root {
  position: relative; width: 100%; height: 100%;
}

/* ----------------------------------------------------------------------------
   [第零层：绝对基石 - 羊皮纸]
   ---------------------------------------------------------------------------- */
.layer-0-parchment {
  position: absolute; inset: 0; z-index: 0;
  background-size: cover; background-position: center;
  /* 略微降低对比度，让上面的墨水更明显 */
  filter: contrast(0.95) brightness(1.05);
}
.micro-noise-overlay {
  position: absolute; inset: 0; z-index: 1; pointer-events: none;
  background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.65" numOctaves="3" stitchTiles="stitch"/></filter><rect width="200" height="200" filter="url(%23n)"/></svg>');
  opacity: 0.06;
}

/* ----------------------------------------------------------------------------
   [第一层：静默侵蚀 - 墨迹叠加]
   !!! 核心技术：mix-blend-mode: multiply !!!
   这会将白底彻底变透明，只留下黑色墨迹和红色血迹融入纸张
   ---------------------------------------------------------------------------- */
.layer-1-corruption { position: absolute; inset: 0; z-index: 5; pointer-events: none; overflow: hidden;}
.ink-asset {
  position: absolute;
  mix-blend-mode: multiply; /* 光学魔法 */
  filter: contrast(1.2); /* 增强墨迹边缘 */
  transition: all 2s ease;
}

.ink-top-left { top: -10%; left: -10%; width: 45vw; opacity: 0.85; }
.ink-bottom-right { bottom: -15%; right: -5%; width: 50vw; opacity: 0.9; transform: rotate(-15deg); }
.sketch-bg-left { top: 20%; left: 5%; width: 40vw; opacity: 0.15; filter: contrast(1) grayscale(100%); } /* 极淡的底图 */
.hand-of-void { bottom: 0; left: 20%; width: 25vw; opacity: 0.7; transform: rotate(15deg) translateY(20px); animation: slight-reach 10s infinite alternate; }

@keyframes slight-reach { 0% { transform: rotate(15deg) translateY(20px); } 100% { transform: rotate(10deg) translateY(-10px); } }

/* ----------------------------------------------------------------------------
   [第二层：Canvas 动态引擎]
   正片叠底，使动态绘制的黑色触手和红色粒子完美融入纸张
   ---------------------------------------------------------------------------- */
.layer-2-canvas {
  position: absolute; inset: 0; z-index: 10; pointer-events: none;
  mix-blend-mode: multiply;
}

/* ----------------------------------------------------------------------------
   [第三层：受难祭坛核心]
   ---------------------------------------------------------------------------- */
.layer-3-altar {
  position: absolute; inset: 0; z-index: 15; pointer-events: none;
  display: flex; justify-content: center; align-items: center;
}
.altar-container {
  position: relative; width: 450px; height: 600px;
  display: flex; justify-content: center; align-items: center;
}
.crucifixion-totem {
  width: 100%; height: auto;
  mix-blend-mode: multiply; /* 剔除白底，保留红色太阳和黑色线条 */
  filter: contrast(1.15) brightness(0.95);
  animation: breathe 6s ease-in-out infinite;
}

/* 中央单词水池读数，浮在耶稣胸口/太阳位置 */
.siphon-core {
  position: absolute; top: 25%; display: flex; flex-direction: column; align-items: center;
  mix-blend-mode: normal; /* 文字需要清晰，不叠底 */
}
.core-ring {
  background: rgba(242, 239, 232, 0.85); border: 2px solid var(--c-text-main);
  border-radius: 50%; width: 140px; height: 140px;
  display: flex; flex-direction: column; justify-content: center; align-items: center;
  box-shadow: 0 0 30px rgba(0,0,0,0.5); backdrop-filter: blur(4px);
}
.vocab-digit { font-family: var(--f-title); font-size: 2.8rem; font-weight: 900; line-height: 1; color: var(--c-blood); }
.vocab-max { font-size: 0.8rem; color: var(--c-text-muted); font-weight: bold; }
.core-label { margin-top: 15px; font-family: var(--f-text); font-size: 1.1rem; font-weight: bold; letter-spacing: 2px; text-shadow: 0 0 5px rgba(255,255,255,0.8); }

/* 狂化时祭坛两侧喷出的鲜血 (墨迹4和5) */
.blood-splash-left { top: 10%; left: -40%; width: 300px; opacity: 0; transform: scale(0.8) translateX(-50px); transition: all 1s cubic-bezier(0.175, 0.885, 0.32, 1.275); mix-blend-mode: multiply; }
.blood-splash-right { bottom: 10%; right: -40%; width: 350px; opacity: 0; transform: scale(0.8) translateX(50px); transition: all 1s cubic-bezier(0.175, 0.885, 0.32, 1.275); mix-blend-mode: multiply; }
.blood-splash-left.active, .blood-splash-right.active { opacity: 0.9; transform: scale(1) translateX(0); }

@keyframes breathe { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.02); } }
.heartbeat { animation: heartbeat-anim 0.8s infinite !important; }
@keyframes heartbeat-anim { 0% { transform: scale(1); } 15% { transform: scale(1.05); } 30% { transform: scale(1); } 45% { transform: scale(1.05); } 100% { transform: scale(1); } }

/* ----------------------------------------------------------------------------
   [第四层：秩序网格 CSS Grid]
   在所有混沌与墨水之上，保持绝对理性的UI层
   ---------------------------------------------------------------------------- */
.layer-4-order-grid {
  position: absolute; inset: 0; z-index: 50;
  padding: 3rem; box-sizing: border-box;
  display: grid;
  grid-template-columns: 350px 1fr 350px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header header"
    "left center right"
    "footer footer footer";
  pointer-events: none; /* 让鼠标事件穿透到下层 Canvas，除了 grid-cell 内部 */
}

.grid-cell { pointer-events: auto; }

/* 天极 */
.header-zone { grid-area: header; display: flex; justify-content: space-between; align-items: flex-start; border-bottom: 2px solid var(--c-text-main); padding-bottom: 1.5rem; }
.artifact-title { font-family: var(--f-title); font-size: clamp(2rem, 3.5vw, 4rem); font-weight: 900; margin: 0; letter-spacing: 4px; }
.artifact-sub { font-family: var(--f-mono); font-size: 0.8rem; font-weight: bold; margin-top: 10px; display: flex; align-items: center; gap: 10px; color: var(--c-text-muted); }
.pulse-node { width: 8px; height: 8px; background: var(--c-blood); border-radius: 50%; animation: pulse-dot 2s infinite; }

.time-block { text-align: right; }
.doom-clock { font-size: clamp(1.8rem, 2.5vw, 3rem); font-weight: 700; line-height: 1; letter-spacing: -1px; }
.doom-label { font-size: 0.75rem; font-weight: bold; color: var(--c-text-muted); margin-top: 8px; letter-spacing: 1px; }

/* 左渊 */
.left-zone { grid-area: left; display: flex; flex-direction: column; padding-top: 2rem; }
.zone-title { font-family: var(--f-text); font-size: 1.4rem; font-weight: bold; border-bottom: 1px solid rgba(0,0,0,0.2); padding-bottom: 5px; margin-bottom: 1.5rem; letter-spacing: 2px; }
.telemetry-matrix { display: flex; flex-direction: column; gap: 1.2rem; font-size: 0.9rem; font-weight: bold; }
.t-row { display: flex; justify-content: space-between; align-items: center; }
.t-key { color: var(--c-text-muted); }
.t-val { font-size: 1.1rem; }
.text-alert { color: var(--c-blood); }
.text-hyper { color: var(--c-blood-bright); font-weight: 900; animation: jitter 0.1s infinite; }

.asset-showcase { margin-top: auto; border: 1px dashed rgba(0,0,0,0.3); padding: 15px; text-align: center; background: rgba(255,255,255,0.1); backdrop-filter: blur(2px); }
.mini-sketch { position: relative; width: 100%; height: auto; mix-blend-mode: multiply; filter: contrast(1.2); }
.sketch-caption { font-family: var(--f-text); font-size: 0.8rem; font-weight: bold; margin: 10px 0 0 0; color: var(--c-text-muted); }

/* 右渊 */
.right-zone { grid-area: right; display: flex; flex-direction: column; padding-top: 2rem; }
.record-scroll { flex: 1; border: 1px solid rgba(0,0,0,0.2); background: rgba(0,0,0,0.03); padding: 1rem; overflow-y: auto; display: flex; flex-direction: column; gap: 0.8rem; font-size: 0.75rem; box-shadow: inset 0 0 15px rgba(0,0,0,0.05); }
/* 滚动条隐藏 */
.record-scroll::-webkit-scrollbar { width: 4px; }
.record-scroll::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.2); }
.log-line { display: flex; gap: 10px; line-height: 1.4; font-weight: bold; }
.log-ts { color: var(--c-text-muted); flex-shrink: 0; }
.log-line.sys .log-msg { color: #333; }
.log-line.echo .log-msg { color: var(--c-text-muted); font-style: italic; }
.log-line.success .log-msg { color: #006600; }
.log-line.alert .log-msg, .log-line.error .log-msg { color: var(--c-blood); }

/* 地极 */
.footer-zone { grid-area: footer; border-top: 2px solid var(--c-text-main); padding-top: 1.5rem; display: flex; align-items: center; }
.terminal-input-wrap { width: 100%; display: flex; align-items: center; gap: 1rem; }
.cli-arrow { font-size: 1.8rem; font-weight: 900; color: var(--c-text-main); }
.cli-input { flex: 1; background: transparent; border: none; outline: none; font-family: var(--f-text); font-size: 1.8rem; font-weight: bold; color: var(--c-text-main); letter-spacing: 1px; }
.cli-input::placeholder { color: var(--c-text-muted); opacity: 0.5; font-style: italic; font-family: var(--f-mono); font-size: 1rem; letter-spacing: 0; }

/* ----------------------------------------------------------------------------
   [系统级特效]
   ---------------------------------------------------------------------------- */
.sanity-break-overlay { position: fixed; inset: 0; z-index: 9999; background: black; display: flex; justify-content: center; align-items: center; animation: break-flash 4s forwards; }
.glitch-huge { font-family: var(--f-title); font-size: 8rem; color: white; animation: text-glitch 0.1s infinite; }

@keyframes pulse-dot { 0%, 100% { opacity: 0.3; transform: scale(1); } 50% { opacity: 1; transform: scale(1.5); } }
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes text-glitch { 0% { transform: translate(3px, -3px); color: red; } 50% { transform: translate(-3px, 3px); color: cyan; } 100% { transform: translate(0,0); color: white; } }
@keyframes break-flash { 0% { opacity: 0; } 10% { opacity: 1; } 90% { opacity: 1; } 100% { opacity: 0; } }

/* 响应式防御 (保证网格在小屏幕缩小但不乱) */
@media (max-width: 1200px) {
  .layer-4-order-grid { grid-template-columns: 250px 1fr 250px; padding: 1.5rem; }
  .crucifixion-totem { width: 80%; }
}
@media (max-width: 900px) {
  .layer-4-order-grid {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto auto auto;
    grid-template-areas: "header" "center" "left" "right" "footer";
    overflow-y: auto; height: 100%; pointer-events: auto;
  }
  .layer-3-altar { position: relative; height: 500px; }
}
</style>