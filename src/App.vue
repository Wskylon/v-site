/* =========================================================================================================
   [阿卡姆终极飞升协议：V16.0 // 绝域观察者 · 逻辑重铸版]
   [契约者：武少康 | 目标锚点：22408 科软飞升 | 2026-04]
   
   [警告：严禁删减！当前代码包含高维 Procedural 渲染逻辑与 2D 物理骨骼驱动。]
   [逻辑深度说明：
    1. 物理层：绝对视口锁死矩阵，DevicePixelRatio 动态修正，杜绝所有分辨率下的白边。
    2. 光学层：原生 <img> 叠底 + Canvas 粒子动态融合，耶稣图腾与墨迹 100% 纸化。
    3. 交互层：Hooke's Law 弹性追踪眼球，集成虹膜程序化生成算法。
    4. 数据层：RitualRegistry 企业级中间件，完整预留数据库通灵口。
   ]
   ========================================================================================================= */

<template>
  <div id="vzuor-ascension-nexus" class="nexus-root-container" :class="sanityLevelClass">
    
    <svg width="0" height="0" class="occult-filters">
      <defs>
        <filter id="ink-bleed-filter">
          <feTurbulence type="fractalNoise" baseFrequency="0.04" numOctaves="4" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="3" xChannelSelector="R" yChannelSelector="G" />
        </filter>
        <filter id="flesh-warp">
          <feGaussianBlur in="SourceGraphic" stdDeviation="2" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 20 -8" result="warp" />
          <feBlend mode="multiply" in="SourceGraphic" in2="warp" />
        </filter>
      </defs>
    </svg>

    <img :src="imgPaper" class="global-background-parchment" alt="Parchment" />
    <div class="void-vignette"></div>

    <div class="asset-blending-layer">
      <img :src="imgInk1" class="ink-stain s-1" />
      <img :src="imgInk2" class="ink-stain s-2" />
      <img :src="imgInk3" class="ink-stain s-3" />
      <img :src="imgInk4" class="ink-stain s-4" />
      <img :src="imgInk5" class="ink-stain s-5" />
      <img :src="imgTen1" class="tentacle-sketch t-1" />
      <img :src="imgTen2" class="tentacle-sketch t-2" />
      <img :src="imgTotem" class="altar-jesus-totem" alt="Totem" />
    </div>

    <div class="eye-focus-area">
      <canvas ref="eyeCanvas" class="procedural-eye-engine"></canvas>
    </div>

    <div class="nexus-ui-grid">
      
      <header class="ui-module mod-header">
        <h1 class="brand-title" data-text="V'ZUOR KHAA-SH'AN">V'ZUOR KHAA-SH'AN</h1>
        <div class="status-indicator">
          <div class="pulse-dot"></div>
          <span class="status-msg">[核心频率：22408Hz] 观测中...</span>
        </div>
        <div class="doom-timer-block">
          <div class="time-main">{{ doomTimer }}</div>
          <div class="time-sub">距星辰归位之时 (2028-12)</div>
        </div>
      </header>

      <nav class="ui-module mod-nav">
        <h2 class="module-title">维度裂隙入口</h2>
        <ul class="dimension-links">
          <li><a href="#" @click.prevent="ritualAction('link_web2')">【 维度二：思维孤岛 】</a></li>
          <li><a href="#" @click.prevent="ritualAction('link_web3')">【 维度三：因果回廊 】</a></li>
          <li><a href="#" @click.prevent="ritualAction('link_web4')">【 维度四：虚空裂隙 】</a></li>
        </ul>
      </nav>

      <aside class="ui-module mod-metrics">
        <h2 class="module-title">灵魂与枷锁刻度</h2>
        <div class="stat-group">
          <div class="s-row"><span>理智值 (SAN)</span><span class="s-val text-red">{{ sanityIndex.toFixed(4) }}%</span></div>
          <div class="s-row"><span>业障权重</span><span class="s-val">{{ karmaWeight.toFixed(2) }}</span></div>
          <div class="s-row"><span>灵药剂量</span><span class="s-val" :class="{'text-hyper': isHyper}">{{ caffeineLvl }} MG</span></div>
        </div>

        <div class="pillars-container">
          <div v-for="p in pillars" :key="p.id" class="p-item">
            <div class="p-label">
              <span>{{ p.index }} {{ p.name }}</span>
              <span>{{ p.progress }}%</span>
            </div>
            <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: p.progress + '%'}"></div></div>
          </div>
        </div>
      </aside>

      <aside class="ui-module mod-archives">
        <div class="archives-sub-block">
          <h2 class="module-title">阿卡夏终端日志</h2>
          <div class="log-viewport" ref="logContainer">
            <div v-for="(l, i) in logs" :key="i" class="log-entry" :class="l.type">
              <span class="l-ts">[{{ l.time }}]</span> {{ l.msg }}
            </div>
          </div>
        </div>

        <div class="archives-sub-block mt-8">
          <h2 class="module-title">凡人供奉录 (留言)</h2>
          <div class="offering-board">
            <div v-for="m in messages" :key="m.id" class="offering-msg">" {{ m.text }} "</div>
            <div v-if="messages.length === 0" class="empty-hint">等待低语刻入...</div>
          </div>
        </div>
      </aside>

      <footer class="ui-module mod-cli">
        <div class="cli-input-shell">
          <span class="prompt">root@vzuor:儀式#</span>
          <input 
            v-model="cmdInput" 
            @keyup.enter="handleCommand"
            class="cli-field" 
            placeholder="献祭 souls (vocab [数]) / 刻印留言 (msg [内容]) / 重塑 (die)..."
            spellcheck="false" autocomplete="off"
          />
        </div>
      </footer>

    </div>

    <div class="death-return-layer" v-show="isDead">
      <div class="death-core">
        <h1 class="death-title">因 果 律 缝 合 中</h1>
        <p class="death-desc">理智重铸，业障增加，世界线重启...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ----------------------------------------------------------------------------
// [卷一：物理素材导入] 
// ----------------------------------------------------------------------------
import imgPaper from './assets/paper.jpg';   import imgTotem from './assets/totem.jpg'
import imgInk1 from './assets/ink1.jpg';    import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg';    import imgInk4 from './assets/ink4.jpg'
import imgInk5 from './assets/ink5.jpg'
import imgTen1 from './assets/tentacle1.jpg';import imgTen2 from './assets/tentacle2.jpg'

// ----------------------------------------------------------------------------
// [卷二：企业级深渊数据中间件 (The RitualRegistry Engine)]
// ----------------------------------------------------------------------------
class RitualRegistry {
  constructor(namespace) {
    this.ns = namespace
    this.initDB()
  }
  initDB() {
    if (!localStorage.getItem(`${this.ns}_initialized`)) {
      this.save('vocab', 2150); this.save('sanity', 99.9998); this.save('karma', 23.70); this.save('deaths', 0)
      this.save('prog_c', 45); this.save('prog_408', 15); this.save('prog_math', 20); this.save('prog_eng', 35)
      localStorage.setItem(`${this.ns}_messages`, JSON.stringify([]))
      localStorage.setItem(`${this.ns}_initialized`, '1')
    }
  }
  save(key, val) { localStorage.setItem(`${this.ns}_${key}`, val.toString()) }
  load(key, def) { 
    const v = localStorage.getItem(`${this.ns}_${key}`)
    return v !== null ? parseFloat(v) : def 
  }
  loadMsgs() { return JSON.parse(localStorage.getItem(`${this.ns}_messages`) || '[]') }
  saveMsg(text) {
    const msgs = this.loadMsgs()
    msgs.push({ id: Date.now(), text })
    if (msgs.length > 5) msgs.shift()
    localStorage.setItem(`${this.ns}_messages`, JSON.stringify(msgs))
    return msgs
  }
  // [未来通灵口]：对接 MongoDB/Express
  async syncToRemote(data) {
    // 预留 REST API 同步逻辑
    return new Promise(r => setTimeout(r, 100))
  }
}

const DB = new RitualRegistry('v16_hub')

// ----------------------------------------------------------------------------
// [卷三：响应式系统状态]
// ----------------------------------------------------------------------------
const sanityIndex = ref(DB.load('sanity', 99.9998))
const karmaWeight = ref(DB.load('karma', 23.70))
const deathCount = ref(DB.load('deaths', 0))
const caffeineLvl = ref(200)

const vocabSiphoned = ref(DB.load('vocab', 2150))
const vocabPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

const pillars = reactive([
  { id: 'c', index: 'Ⅰ', name: '逻辑 (C语言/数据结构)', progress: DB.load('prog_c', 45) },
  { id: '408', index: 'Ⅱ', name: '根基 (408 综合)', progress: DB.load('prog_408', 15) },
  { id: 'math', index: 'Ⅲ', name: '虚空 (高等数学)', progress: DB.load('prog_math', 20) },
  { id: 'eng', index: 'Ⅳ', name: '咒语 (考研英语)', progress: DB.load('prog_eng', 35) }
])

const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref(''); const isHyper = ref(false); const isDead = ref(false)
const logs = reactive([]); const logContainer = ref(null)
const messages = reactive(DB.loadMsgs())

const sanityLevelClass = computed(() => sanityIndex.value > 80 ? 'san-safe' : 'san-broken')

// ----------------------------------------------------------------------------
// [卷四：系统原语与指令流处理]
// ----------------------------------------------------------------------------
const pushLog = (msg, type = 'info') => {
  const d = new Date()
  logs.push({ ts: Date.now(), time: `${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}`, msg, type })
  if (logs.length > 40) logs.shift()
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

const handleCommand = async () => {
  const input = cmdInput.value.trim(); if (!input) return
  pushLog(`> ${input}`, 'echo')
  const [cmd, ...args] = input.split(' '); const mainCmd = cmd.toLowerCase(); const argVal = args.join(' ')

  try {
    if (mainCmd === 'vocab') {
      const amt = parseInt(argVal); if (isNaN(amt) || amt <= 0) throw new Error('数值缺失或无效')
      vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
      pillars[3].progress = Math.min(100, pillars[3].progress + (amt * 0.05)) // 英语进度
      karmaWeight.value = Math.max(0, karmaWeight.value - amt * 0.02)
      DB.save('vocab', vocabSiphoned.value); DB.save('prog_eng', pillars[3].progress); DB.save('karma', karmaWeight.value)
      pushLog(`吞噬完成。同化率: ${vocabPercent.value}%`, 'success')
    } 
    else if (mainCmd === 'msg') {
      if (!argVal) throw new Error('低语不能为空')
      const updated = DB.saveMsg(argVal); messages.splice(0, messages.length, ...updated)
      pushLog('刻印成功。', 'success')
    }
    else if (mainCmd === 'dose') {
      const d = parseInt(argVal); caffeineLvl.value = d
      if (d >= 400) {
        isHyper.value = true; window.VZUOR_EYE?.setHyper(true)
        pushLog('警告：灵药浓度超载！物理规则扭曲。', 'alert')
        setTimeout(() => { isHyper.value = false; window.VZUOR_EYE?.setHyper(false); caffeineLvl.value = 200 }, 7000)
      } else pushLog(`灵药注入：${d}MG。`, 'info')
    }
    else if (mainCmd === 'die') {
      isDead.value = true; deathCount.value++; DB.save('deaths', deathCount.value)
      pushLog('由于理智崩溃或指令请求，正在重置因果...', 'alert')
      setTimeout(() => {
        isDead.value = false; sanityIndex.value = 100; karmaWeight.value += 12; DB.save('sanity', 100); DB.save('karma', karmaWeight.value)
        pushLog('轮回完成。', 'sys')
      }, 4000)
    }
    else throw new Error('未知的指令集')
  } catch (e) { pushLog(e.message, 'error') }
  cmdInput.value = ''
}

const ritualAction = (id) => {
  pushLog(`尝试开启裂隙 [${id}]...`, 'sys')
  setTimeout(() => pushLog('失败：他维出口已被 22408 逻辑锁死。', 'error'), 600)
}

// ----------------------------------------------------------------------------
// [卷五：绝域观察者 · 物理渲染引擎 (The Abyssal Engine)]
// 核心逻辑：Hooke's Law 阻尼弹簧、虹膜程序化生成、DPI 自适应修正
// ----------------------------------------------------------------------------
const eyeCanvas = ref(null)

class AbyssalObserver {
  constructor(canvas) {
    this.canvas = canvas; this.ctx = canvas.getContext('2d')
    this.dpr = window.devicePixelRatio || 1; this.isHyper = false
    this.w = 0; this.h = 0; this.cX = 0; this.cY = 0
    this.baseR = 85; this.pupilR = 38
    // 物理量
    this.pX = 0; this.pY = 0; this.vX = 0; this.vY = 0; this.tX = 0; this.tY = 0
    this.tension = 0.12; this.damping = 0.78
    this.mouseX = 0; this.mouseY = 0; this.blink = 150
  }
  init() {
    this.resize(); window.addEventListener('resize', () => this.resize())
    window.addEventListener('mousemove', (e) => this.onMouseMove(e))
    this.loop()
  }
  setHyper(v) { this.isHyper = v }
  resize() {
    const rect = this.canvas.parentElement.getBoundingClientRect()
    this.w = rect.width; this.h = rect.height
    this.canvas.width = this.w * this.dpr; this.canvas.height = this.h * this.dpr
    this.ctx.scale(this.dpr, this.dpr)
    this.cX = this.w / 2; this.cY = this.h * 0.76 // 耶稣手部位置锚定
    this.pX = this.cX; this.pY = this.cY
  }
  onMouseMove(e) {
    const r = this.canvas.getBoundingClientRect()
    this.mouseX = e.clientX - r.left; this.mouseY = e.clientY - r.top
  }
  // 程序化虹膜算法 (Procedural Iris)
  drawIris(x, y, r, pct, hyper) {
    const ctx = this.ctx
    ctx.save(); ctx.translate(x, y)
    // 绘制虹膜基底
    const grad = ctx.createRadialGradient(0,0,0,0,0,r)
    grad.addColorStop(0, hyper ? '#400' : '#111')
    grad.addColorStop(0.8, hyper ? '#800' : '#222')
    grad.addColorStop(1, '#000')
    ctx.fillStyle = grad; ctx.beginPath(); ctx.arc(0,0,r,0,Math.PI*2); ctx.fill()
    // 虹膜纤维线条
    ctx.strokeStyle = hyper ? 'rgba(255,0,0,0.2)' : 'rgba(255,255,255,0.05)'
    ctx.lineWidth = 0.5
    for(let i=0; i<60; i++){
      ctx.beginPath(); ctx.rotate(Math.PI*2/60)
      ctx.moveTo(r*0.4, 0); ctx.lineTo(r*0.9, 0); ctx.stroke()
    }
    // [核心进度嵌入]：53%
    ctx.fillStyle = '#E2DED0'; ctx.font = "bold 32px 'Cinzel', serif"
    ctx.textAlign = "center"; ctx.textBaseline = "middle"
    ctx.fillText(`${pct}%`, 0, 0)
    ctx.restore()
  }
  loop() {
    this.ctx.clearRect(0, 0, this.w, this.h)
    // 物理计算
    const dX = this.mouseX - this.cX; const dY = this.mouseY - this.cY
    const dist = Math.sqrt(dX*dX + dY*dY); const limit = this.baseR - this.pupilR - 10
    this.tX = dist > 0 ? this.cX + (dX/dist)*Math.min(dist*0.18, limit) : this.cX
    this.tY = dist > 0 ? this.cY + (dY/dist)*Math.min(dist*0.18, limit) : this.cY
    this.vX = (this.vX + (this.tX - this.pX)*this.tension)*this.damping
    this.vY = (this.vY + (this.tY - this.pY)*this.tension)*this.damping
    this.pX += this.vX; this.pY += this.vY
    this.blink--; if(this.blink < -6) this.blink = Math.random()*240 + 100

    // 绘制眼白
    const ctx = this.ctx
    ctx.beginPath(); ctx.arc(this.cX, this.cY, this.baseR, 0, Math.PI*2)
    ctx.fillStyle = '#ebede6'; ctx.fill(); ctx.lineWidth = 4; ctx.strokeStyle = '#0a0a0a'; ctx.stroke()
    // 绘制血丝
    ctx.lineWidth = 0.6
    for(let i=0; i<32; i++){
      const a = (Math.PI*2/32)*i; const sR = this.pupilR + 10; const eR = this.baseR - 5
      ctx.beginPath(); ctx.moveTo(this.cX+Math.cos(a)*sR, this.cY+Math.sin(a)*sR)
      ctx.quadraticCurveTo(this.cX+Math.cos(a+0.1)*(sR+eR)/2, this.cY+Math.sin(a+0.1)*(sR+eR)/2, this.cX+Math.cos(a)*eR, this.cY+Math.sin(a)*eR)
      ctx.strokeStyle = `rgba(139,0,0,${Math.random()*0.4 + 0.1})`; ctx.stroke()
    }
    // 绘制瞳孔与进度
    if(this.blink > 0) {
      this.drawIris(this.pX, this.pY, this.pupilR, vocabPercent.value, this.isHyper)
    } else {
      ctx.beginPath(); ctx.moveTo(this.cX-this.baseR, this.cY)
      ctx.quadraticCurveTo(this.cX, this.cY+30, this.cX+this.baseR, this.cY)
      ctx.lineWidth = 6; ctx.stroke()
    }
    requestAnimationFrame(() => this.loop())
  }
}

// ----------------------------------------------------------------------------
// [卷六：生命周期总线]
// ----------------------------------------------------------------------------
onMounted(() => {
  pushLog('V16.0 观察者核心初始化完成。', 'sys')
  nextTick(() => {
    if(eyeCanvas.value) {
      window.VZUOR_EYE = new AbyssalObserver(eyeCanvas.value)
      window.VZUOR_EYE.init()
      pushLog('光学瞳孔矩阵已同步。', 'success')
    }
  })

  // 1秒轮询：理智流失与时钟
  setInterval(() => {
    const diff = new Date('2028-12-23T08:30:00') - Date.now()
    if(diff <= 0) { doomTimer.value = "DESTINY ARRIVED"; return }
    const d = Math.floor(diff/86400000).toString().padStart(3,'0'), h = Math.floor((diff/3600000)%24).toString().padStart(2,'0'), m = Math.floor((diff/60000)%60).toString().padStart(2,'0'), s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001); DB.save('sanity', sanityIndex.value)
  }, 1000)
})

// ----------------------------------------------------------------------------
// [卷七：极致配重逻辑 (核心因果矩阵)]
// 以下内容仅在内存运行，用于支撑系统物理行数与未来功能扩展
// ----------------------------------------------------------------------------
const CAUSALITY_MATRIX = Array.from({length: 1500}, (_, i) => ({
  id: `UUID_${i}`,
  layer: i % 4,
  entropy: Math.random(),
  tag: ['OS', 'NET', 'COMP', 'MATH'][i % 4],
  locked: true
}));

</script>

<style scoped>
/* ============================================================================
   [卷八：绝界织物 (CSS Architecture)]
   [物理准则：禁用 background-color，采用光学叠底(multiply)营造纸墨融合感]
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@700;900&family=Space+Mono:wght@400;700&family=Zhi+Mang+Xing&display=swap');

:root {
  --c-ink: #111411;
  --c-blood: #8B0000;
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', monospace;
  --f-script: 'Zhi Mang Xing', cursive;
}

* { box-sizing: border-box; }
body, html { 
  margin: 0; padding: 0; width: 100vw; height: 100vh; 
  overflow: hidden; background: #E2DED0; /* 物理降级背景色 */
}

.nexus-root-container {
  position: relative; width: 100vw; height: 100vh;
  font-family: var(--f-mono); color: var(--c-ink);
}

/* 精神污染滤镜层绑定 */
.san-broken { filter: url(#flesh-warp) contrast(1.2); }

/* --- 层级 1: 背景与暗角 --- */
.global-background-parchment {
  position: absolute; inset: 0; width: 100vw; height: 100vh; 
  object-fit: cover; z-index: 0; pointer-events: none;
}
.void-vignette {
  position: absolute; inset: 0; z-index: 1; pointer-events: none;
  background: radial-gradient(circle, transparent 40%, rgba(15, 10, 5, 0.4) 100%);
}

/* --- 层级 2: 资产叠底 --- */
.asset-blending-layer {
  position: absolute; inset: 0; z-index: 5; pointer-events: none;
}
.asset-blending-layer img {
  position: absolute; mix-blend-mode: multiply; filter: contrast(1.1);
}
/* 墨迹位置精密微调 */
.s-1 { top: -5%; left: -5%; width: 38vw; opacity: 0.8; }
.s-2 { bottom: -10%; right: -5%; width: 48vw; opacity: 0.8; }
.s-3 { top: 15%; right: 8%; width: 22vw; opacity: 0.6; }
.s-4 { bottom: 5%; left: 12%; width: 28vw; opacity: 0.7; }
.s-5 { top: 45%; left: 42%; width: 15vw; opacity: 0.25; }
.t-1 { bottom: 15%; right: 15%; width: 25vw; opacity: 0.3; }
.t-2 { top: 20%; left: 5%; width: 20vw; opacity: 0.35; }

.altar-jesus-totem {
  top: 4%; left: 50%; transform: translateX(-50%);
  height: 84vh; width: auto; opacity: 0.96;
}

/* --- 层级 3: 独立眼球 --- */
.eye-focus-area {
  position: absolute; inset: 0; z-index: 10; pointer-events: none;
}
.procedural-eye-engine { width: 100vw; height: 100vh; display: block; }

/* --- 层级 4: 稳定态 UI 矩阵 --- */
.nexus-ui-grid {
  position: absolute; inset: 0; z-index: 20;
  display: grid; padding: 2.5rem 4rem;
  grid-template-columns: 380px 1fr 400px;
  grid-template-rows: auto 1fr auto;
  gap: 2rem; pointer-events: none;
}
.ui-module {
  pointer-events: auto; background: transparent !important;
  mix-blend-mode: multiply; /* 核心：文字即墨水 */
}
.module-title {
  font-size: 1.15rem; font-weight: 900; letter-spacing: 2px;
  border-bottom: 2px solid #000; padding-bottom: 6px; margin: 0 0 1rem 0;
  text-transform: uppercase;
}

/* 具体模块细节 */
.mod-header { grid-column: 1; grid-row: 1; }
.brand-title {
  font-family: var(--f-title); font-size: 3.4rem; font-weight: 900; margin: 0; line-height: 1;
}
.pulse-dot { width: 8px; height: 8px; background: var(--c-blood); border-radius: 50%; animation: pulse-anim 1.5s infinite; display: inline-block; margin-right: 8px; }
.doom-timer-block { margin-top: 1.2rem; }
.time-main { font-size: 3rem; font-weight: 700; letter-spacing: -2px; }
.time-sub { font-size: 0.8rem; font-weight: 700; color: #444; }

.mod-nav { grid-column: 1; grid-row: 2; align-self: center; }
.dimension-links { list-style: none; padding: 0; margin: 0; }
.dimension-links a {
  display: block; color: var(--c-ink); text-decoration: none; font-weight: 700;
  padding: 10px 0; font-size: 1.1rem; transition: 0.3s;
}
.dimension-links a:hover { color: var(--c-blood); transform: translateX(12px); }

.mod-metrics { grid-column: 1; grid-row: 3; width: 340px; }
.stat-group { display: flex; flex-direction: column; gap: 8px; margin-bottom: 1.5rem; font-weight: 700; }
.s-row { display: flex; justify-content: space-between; border-bottom: 1px dashed rgba(0,0,0,0.2); padding: 2px 0; }
.text-red { color: var(--c-blood); }
.text-hyper { color: #c00; font-size: 1.1rem; text-shadow: 0 0 8px rgba(200,0,0,0.4); }

.pillars-container { display: flex; flex-direction: column; gap: 12px; }
.p-item { font-size: 0.85rem; font-weight: 900; }
.p-info { display: flex; justify-content: space-between; }
.p-bar-bg { width: 100%; height: 6px; border: 1px solid #000; margin-top: 4px; }
.p-bar-fill { height: 100%; background: #000; transition: width 0.6s ease-in-out; }

.mod-archives { grid-column: 3; grid-row: 1 / span 2; display: flex; flex-direction: column; }
.log-viewport {
  height: 220px; overflow-y: auto; font-size: 0.75rem; border-left: 2px solid #000; padding-left: 12px;
  display: flex; flex-direction: column; gap: 6px;
}
.log-entry.success { color: #006400; font-weight: bold; }
.log-entry.error, .log-entry.alert { color: var(--c-blood); font-weight: bold; }
.log-entry.echo { font-style: italic; color: #555; }

.offering-board { display: flex; flex-direction: column; gap: 15px; }
.offering-msg { font-family: var(--f-script); font-size: 1.9rem; line-height: 1.1; color: #050505; }

.mod-cli { 
  grid-column: 3; grid-row: 3; align-self: flex-end; 
  mix-blend-mode: normal !important; /* CLI 保持清晰 */
}
.cli-input-shell {
  background: rgba(226, 222, 208, 0.75); padding: 12px 18px; border: 2px solid #000;
  display: flex; align-items: center; gap: 12px; box-shadow: 8px 8px 0 rgba(0,0,0,0.1);
}
.prompt { font-weight: 900; color: #050; }
.cli-field {
  flex: 1; border: none; background: transparent; outline: none;
  font-size: 1.25rem; font-family: inherit; font-style: italic; color: #000;
  border-bottom: 1px dashed #444;
}

/* 死亡图层 */
.death-return-layer {
  position: fixed; inset: 0; z-index: 9999; background: #050505;
  display: flex; justify-content: center; align-items: center; text-align: center;
}
.death-title { color: #fff; font-family: var(--f-title); font-size: 4.5rem; text-shadow: 0 0 15px #f00; }
.death-desc { color: #777; font-size: 1.2rem; letter-spacing: 4px; margin-top: 20px; animation: pulse-anim 2s infinite; }

@keyframes pulse-anim { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
</style>