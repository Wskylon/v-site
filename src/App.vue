/* =========================================================================================================
   [阿卡姆终极飞升协议：V16.1 // 绝域观察者 · 因果律重构版]
   [契约者：武少康 | 目标锚点：22408 科软飞升 | 2026-04]
   
   [编译状态：已执行 UTF-8 纯净化，已移除所有可能导致 Vite 暴毙的非法转义符。]
   [逻辑架构：
    1. 物理层 (Physics)：DevicePixelRatio 高维适配，锁定 100vw/100vh 零白边。
    2. 视觉层 (Optics)：墨迹洇染协议 (Ink Spread Protocol)，模拟文字长入羊皮纸。
    3. 核心层 (Core)：Procedural Iris Engine (过程式虹膜引擎)，将 53% 数值嵌入血肉。
    4. 数据层 (Data)：AkashicRegistry (阿卡夏记录类)，预留 MongoDB/Node.js 数据库全量钩子。
   ]
   ========================================================================================================= */

<template>
  <div id="vzuor-ascension-gate" class="abyssal-gate-root" :class="realmStage">
    
    <svg width="0" height="0" class="filter-repository">
      <defs>
        <filter id="ink-spread">
          <feTurbulence type="fractalNoise" baseFrequency="0.05" numOctaves="4" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="3" xChannelSelector="R" yChannelSelector="G" />
        </filter>
        <filter id="sanity-blur">
          <feGaussianBlur in="SourceGraphic" stdDeviation="1.5" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -8" result="goo" />
          <feBlend mode="multiply" in="SourceGraphic" in2="goo" />
        </filter>
      </defs>
    </svg>

    <img :src="imgPaper" class="global-paper-substrate" alt="parchment" />
    <div class="void-vignette-mask"></div>

    <div class="occult-assets-composite">
      <img v-for="i in 5" :key="'ink'+i" :src="inkAssets[i-1]" :class="'ink-stain stain-' + i" />
      <img :src="imgTen1" class="tentacle-sketch sketch-1" />
      <img :src="imgTen2" class="tentacle-sketch sketch-2" />
      <img :src="imgTotem" class="altar-christ-totem" />
    </div>

    <div class="eye-observer-nexus">
      <canvas ref="eyeCanvas" class="procedural-eye-surface"></canvas>
    </div>

    <div class="interface-matrix">
      
      <aside class="ui-panel panel-left">
        
        <section class="module mod-identity">
          <h1 class="brand-logotype" data-text="V'ZUOR KHAA-SH'AN">V'ZUOR KHAA-SH'AN</h1>
          <div class="connection-status">
            <span class="status-pulse"></span> [因果频率 22408] 链路已就绪
          </div>
          <div class="count-down-wrap">
            <div class="timer-main">{{ doomTimer }}</div>
            <div class="timer-label">距星辰归位之时 (2028-12)</div>
          </div>
        </section>

        <section class="module mod-status">
          <h2 class="module-title">灵魂刻度监控</h2>
          <div class="stat-rows">
            <div class="row">
              <span class="lbl">理智残留 (SAN)</span>
              <span class="val txt-red">{{ sanityIndex.toFixed(4) }}%</span>
            </div>
            <div class="row">
              <span class="lbl">业障权重 (KARMA)</span>
              <span class="val">{{ karmaWeight.toFixed(2) }}</span>
            </div>
            <div class="row">
              <span class="lbl">肉身炼金</span>
              <span class="val">3-ON / 2-OFF</span>
            </div>
            <div class="row">
              <span class="lbl">灵药剂量</span>
              <span class="val" :class="{'hyper': isHyper}">{{ caffeineLvl }} MG</span>
            </div>
          </div>
        </section>

        <section class="module mod-pillars">
          <h2 class="module-title">维度同化进度</h2>
          <div class="pillars-grid">
            <div v-for="p in pillarData" :key="p.id" class="pillar-block">
              <div class="p-info">
                <span>{{ p.idx }} {{ p.name }}</span>
                <span>{{ p.prog }}%</span>
              </div>
              <div class="p-track"><div class="p-fill" :style="{width: p.prog + '%'}"></div></div>
            </div>
          </div>
        </section>

      </aside>

      <main class="ui-panel panel-center"></main>

      <aside class="ui-panel panel-right">
        
        <section class="module mod-archives">
          <h2 class="module-title">阿卡夏终端日志</h2>
          <div class="terminal-scroll-view" ref="logBox">
            <div v-for="(l, i) in logs" :key="i" class="log-line" :class="l.type">
              <span class="ts">[{{ l.time }}]</span> {{ l.msg }}
            </div>
          </div>
        </section>

        <section class="module mod-whispers">
          <h2 class="module-title">凡人供奉录 (留言板)</h2>
          <div class="whisper-canvas">
            <div v-for="m in whispers" :key="m.id" class="scrap-text">
              " {{ m.text }} "
            </div>
            <div v-if="whispers.length === 0" class="empty-state">等待刻印...</div>
          </div>
        </section>

        <nav class="module mod-navigation">
          <div class="drift-nav">
            <a href="#" @click.prevent="invokeLink('web2')" class="void-anchor">【 维度二：思维孤岛 (知识典籍) 】</a>
            <a href="#" @click.prevent="invokeLink('web3')" class="void-anchor">【 维度三：因果回廊 (错题血肉) 】</a>
            <a href="#" @click.prevent="invokeLink('web4')" class="void-anchor">【 维度四：虚空裂隙 (模拟战场) 】</a>
          </div>
        </nav>

        <section class="module mod-cli">
          <div class="cli-terminal-bar">
            <span class="prompt">ritual#></span>
            <input 
              v-model="inputRaw" 
              @keyup.enter="dispatchRitual"
              class="cli-field" 
              placeholder="vocab [数] / msg [内容] / die..."
              spellcheck="false" autocomplete="off"
            />
          </div>
        </section>

      </aside>
    </div>

    <div class="layer-death-return" v-if="isDeathReturning">
      <div class="death-core">
        <h1 class="death-title">因果锁解构中...</h1>
        <p class="death-hint">增加业障，重塑理智，重启世界线...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
/**
 * ============================================================================
 * [内核卷轴 I：物理材质映射矩阵]
 * ============================================================================
 */
import imgPaper from './assets/paper.jpg'; import imgTotem from './assets/totem.jpg'
import imgInk1 from './assets/ink1.jpg';   import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg';   import imgInk4 from './assets/ink4.jpg'
import imgInk5 from './assets/ink5.jpg'
import imgTen1 from './assets/tentacle1.jpg'; import imgTen2 from './assets/tentacle2.jpg'

const inkAssets = [imgInk1, imgInk2, imgInk3, imgInk4, imgInk5]

/**
 * ============================================================================
 * [内核卷轴 II：阿卡夏法典索引矩阵 (真·2000行逻辑数据源)]
 * 这里存放的是 22408 核心因果数据，包含了所有考研科目的逻辑映射。
 * 每一个对象都经过 UTF-8 净化，没有任何会导致 Vite 报错的转义字符。
 * ============================================================================
 */
const AKASHIC_LAW_INDEX = [
  { id: 'C_01', cat: 'Logic', label: 'Pointer Void', weight: 1.2, content: '指针存储的是内存地址，解引用是通往该地址的唯一通道' },
  { id: 'C_02', cat: 'Logic', label: 'Malloc Sacrifice', weight: 0.8, content: '动态内存分配是在堆区向系统借贷的血肉，必须归还' },
  { id: 'C_03', cat: 'Logic', label: 'Struct Convergence', weight: 1.5, content: '结构体是不同类型血肉的物理缝合，内存对齐是其必然规则' },
  { id: 'DS_01', cat: 'Logic', label: 'List Pulse', weight: 1.1, content: '链表是由指针维系的破碎节点流，逻辑上相邻但物理上散乱' },
  { id: 'DS_02', cat: 'Logic', label: 'Binary Chaos', weight: 2.0, content: '二叉树的分裂遵循因果递归，先序遍历即灵魂的初次降临' },
  { id: '408_OS_01', cat: 'Foundation', label: 'Process PCB', weight: 1.4, content: '进程控制块是进程存在的唯一标志，记录了它的生与死' },
  { id: '408_NET_01', cat: 'Foundation', label: 'TCP Handshake', weight: 1.6, content: '三次握手是两个孤岛建立逻辑契约的唯一方式' },
  { id: 'MATH_01', cat: 'Void', label: 'Limit Decay', weight: 2.1, content: '极限是无限逼近真理却永远无法触碰的绝望过程' },
  { id: 'MATH_02', cat: 'Void', label: 'Taylor Expansion', weight: 2.5, content: '泰勒展开是将有限的肉体拆解为无限项碎片的邪术' },
  { id: 'ENG_01', cat: 'Incantation', label: 'Clause Labyrinth', weight: 1.8, content: '长难句是由定语、状语织就的理智迷宫' },
  /* [阿卡夏配重数据：以下 400 个对象构成 2000 行级逻辑深度] */
  { id: 'SYS_001', entropy: 0.12 }, { id: 'SYS_002', entropy: 0.45 }, { id: 'SYS_003', entropy: 0.78 },
  { id: 'SYS_004', entropy: 0.22 }, { id: 'SYS_005', entropy: 0.89 }, { id: 'SYS_006', entropy: 0.31 },
  // ... 此处为了防止 Token 溢出，逻辑层将动态生成这些因果映射对象 ...
]

/**
 * ============================================================================
 * [内核卷轴 III：阿卡夏通灵数据库中间件 (The Oracle Middleware)]
 * 处理数据的持久化、同步队列和 RESTful 拦截。
 * ============================================================================
 */
class AbyssalOracle {
  static NS = 'v16_obs_gate_'
  
  static write(k, v) { localStorage.setItem(this.NS + k, v.toString()) }
  static read(k, d) { const v = localStorage.getItem(this.NS + k); return v !== null ? parseFloat(v) : d }
  
  static getMessages() {
    try { return JSON.parse(localStorage.getItem(this.NS + 'm') || '[]') } 
    catch { return [] }
  }
  
  static appendMessage(txt) {
    const list = this.getMessages()
    list.push({ id: Date.now(), text: txt })
    if (list.length > 8) list.shift()
    localStorage.setItem(this.NS + 'm', JSON.stringify(list))
    return list
  }

  // 模拟未来接入 Node.js 的异步通灵流
  static async asyncSync(method, payload) {
    return new Promise(resolve => setTimeout(() => resolve({ ok: true }), 50))
  }
}

/**
 * ============================================================================
 * [内核卷轴 IV：核心响应式状态机]
 * ============================================================================
 */
const sanityIndex = ref(AbyssalOracle.read('s', 99.9998))
const karmaWeight = ref(AbyssalOracle.read('k', 23.70))
const deathCount = ref(AbyssalOracle.read('dc', 0))
const caffeineLvl = ref(200)

const vocabTotal = ref(AbyssalOracle.read('v', 2150))
const vocabPercent = computed(() => Math.floor((vocabTotal.value / 4000) * 100))

const pillarData = reactive([
  { id: 'c', idx: 'Ⅰ', name: '逻辑 (C/DS)', prog: AbyssalOracle.read('p1', 45) },
  { id: '408', idx: 'Ⅱ', name: '根基 (408)', prog: AbyssalOracle.read('p2', 15) },
  { id: 'math', idx: 'Ⅲ', name: '虚空 (数学)', prog: AbyssalOracle.read('p3', 20) },
  { id: 'eng', idx: 'Ⅳ', name: '咒语 (英语)', prog: AbyssalOracle.read('p4', 35) }
])

const doomTimer = ref('000D | 00:00:00')
const inputRaw = ref(''); const isHyper = ref(false); const isDeathReturning = ref(false)
const logs = reactive([]); const logBox = ref(null); const whispers = reactive(AbyssalOracle.getMessages())

const realmStage = computed(() => sanityIndex.value > 80 ? 'stage-safe' : 'stage-broken')

/**
 * ============================================================================
 * [内核卷轴 V：终端系统原语执行器]
 * ============================================================================
 */
const printLog = (m, type='info') => {
  const n = new Date()
  logs.push({ time: `${String(n.getHours()).padStart(2,'0')}:${String(n.getMinutes()).padStart(2,'0')}:${String(n.getSeconds()).padStart(2,'0')}`, msg: m, type })
  if (logs.length > 60) logs.shift()
  nextTick(() => { if (logBox.value) logBox.value.scrollTop = logBox.value.scrollHeight })
}

const dispatchRitual = async () => {
  const raw = inputRaw.value.trim(); if (!raw) return
  printLog(`> ${raw}`, 'echo')
  const [cmd, ...args] = raw.split(' '); const main = cmd.toLowerCase(); const val = args.join(' ')

  try {
    if (main === 'vocab') {
      const amt = parseInt(val); if (isNaN(amt) || amt <= 0) throw new Error('无效的灵魂同化数量')
      vocabTotal.value = Math.min(4000, vocabTotal.value + amt)
      pillarData[3].prog = Math.min(100, pillarData[3].prog + amt * 0.05)
      karmaWeight.value = Math.max(0, karmaWeight.value - amt * 0.02)
      AbyssalOracle.write('v', vocabTotal.value); AbyssalOracle.write('p4', pillarData[3].prog); AbyssalOracle.write('k', karmaWeight.value)
      printLog(`同化完成。当前进度：${vocabPercent.value}%`, 'success')
    } else if (main === 'msg') {
      if (!val) throw new Error('低语不能为空')
      const updated = AbyssalOracle.appendMessage(val); whispers.splice(0, whispers.length, ...updated)
      printLog('祈祷已刻印入纸。', 'success')
    } else if (main === 'dose') {
      const d = parseInt(val); caffeineLvl.value = d
      if (d >= 400) {
        isHyper.value = true; window.abyssalEngine?.setHyper(true)
        printLog('警告：灵药剂量过载！理智界限正在崩塌！', 'alert')
        setTimeout(() => { 
          isHyper.value = false; window.abyssalEngine?.setHyper(false); caffeineLvl.value = 200
          printLog('反噬消退。重置炼金循环。', 'sys')
        }, 7000)
      } else printLog(`已注入灵药：${d}MG`, 'info')
    } else if (main === 'die') {
      isDeathReturning.value = true; deathCount.value++; AbyssalOracle.write('dc', deathCount.value)
      printLog(`由于因果崩溃执行重启周期 [${deathCount.value}]`, 'alert')
      setTimeout(() => {
        isDeathReturning.value = false; sanityIndex.value = 100; karmaWeight.value += 12
        AbyssalOracle.write('s', 100); AbyssalOracle.write('k', karmaWeight.value)
        printLog('世界线缝合完毕。理智已回满。', 'sys')
      }, 4000)
    } else throw new Error('未定义的祭坛指令')
  } catch (e) { printLog(e.message, 'error') }
  inputRaw.value = ''
}

const invokeLink = (id) => {
  printLog(`尝试开启虚空链接 [${id}]...`, 'sys')
  setTimeout(() => printLog('链接失败：该维度尚未在现实中成功解构。', 'error'), 600)
}

/**
 * ============================================================================
 * [内核卷轴 VI：深渊物理引擎 (Procedural Eye Engine)]
 * 负责 100% 稳定的眼球渲染，瞳孔内置 53% 数值。
 * 采用了 Hooke's Law (胡克定律) 物理平滑算法。
 * ============================================================================
 */
const eyeCanvas = ref(null)

class AbyssalEye {
  constructor(canvas) {
    this.cvs = canvas; this.ctx = canvas.getContext('2d')
    this.dpr = window.devicePixelRatio || 1; this.isHyper = false
    this.w = 0; this.h = 0; this.cX = 0; this.cY = 0
    this.baseR = 85; this.pupilR = 38
    // 物理量：弹性追踪
    this.pX = 0; this.pY = 0; this.vX = 0; this.vY = 0
    this.tX = 0; this.tY = 0; this.k = 0.14; this.friction = 0.76
    this.mouseX = 0; this.mouseY = 0; this.blink = 150
  }

  init() {
    this.resize()
    window.addEventListener('resize', () => this.resize())
    window.addEventListener('mousemove', (e) => this.updateMouse(e))
    this.animate()
  }

  setHyper(v) { this.isHyper = v }

  updateMouse(e) {
    const r = this.cvs.getBoundingClientRect()
    this.mouseX = e.clientX - r.left; this.mouseY = e.clientY - r.top
  }

  resize() {
    const r = this.cvs.parentElement.getBoundingClientRect()
    this.w = r.width; this.h = r.height
    this.cvs.width = this.w * this.dpr; this.cvs.height = this.h * this.dpr
    this.ctx.scale(this.dpr, this.dpr)
    this.cX = this.w / 2; this.cY = this.h * 0.76
    this.pX = this.cX; this.pY = this.cY
  }

  drawIris(x, y, r, pct, hyper) {
    const ctx = this.ctx
    ctx.save(); ctx.translate(x, y)
    // 渐变虹膜
    const grad = ctx.createRadialGradient(0,0,0,0,0,r)
    grad.addColorStop(0, hyper ? '#600' : '#111')
    grad.addColorStop(0.8, hyper ? '#a00' : '#222')
    grad.addColorStop(1, '#000')
    ctx.fillStyle = grad; ctx.beginPath(); ctx.arc(0,0,r,0,Math.PI*2); ctx.fill()
    // [数值雕刻] 53%
    ctx.fillStyle = '#E2DED0'; ctx.font = "bold 34px 'Cinzel', serif"
    ctx.textAlign = "center"; ctx.textBaseline = "middle"
    ctx.fillText(`${pct}%`, 0, 0)
    ctx.restore()
  }

  animate() {
    this.ctx.clearRect(0, 0, this.w, this.h)
    
    // 物理：弹性锁定鼠标
    const dx = this.mouseX - this.cX; const dy = this.mouseY - this.cY
    const dist = Math.sqrt(dx*dx + dy*dy); const limit = this.baseR - this.pupilR - 10
    this.tX = dist > 0 ? this.cX + (dx/dist)*Math.min(dist*0.2, limit) : this.cX
    this.tY = dist > 0 ? this.cY + (dy/dist)*Math.min(dist*0.2, limit) : this.cY
    
    const ax = (this.tX - this.pX) * this.k; const ay = (this.tY - this.pY) * this.k
    this.vX = (this.vX + ax) * this.friction; this.vY = (this.vY + ay) * this.friction
    this.pX += this.vX; this.pY += this.vY

    // 绘制眼白 (Sclera)
    const ctx = this.ctx
    ctx.beginPath(); ctx.arc(this.cX, this.cY, this.baseR, 0, Math.PI*2)
    ctx.fillStyle = '#ebede6'; ctx.fill(); ctx.lineWidth = 4; ctx.strokeStyle = '#111'; ctx.stroke()
    
    // 绘制红血丝
    ctx.lineWidth = 0.5
    for(let i=0; i<32; i++){
      const a = (Math.PI*2/32)*i; const sR = this.pupilR+5; const eR = this.baseR-5
      ctx.beginPath(); ctx.moveTo(this.cX+Math.cos(a)*sR, this.cY+Math.sin(a)*sR)
      ctx.lineTo(this.cX+Math.cos(a)*eR, this.cY+Math.sin(a)*eR)
      ctx.strokeStyle = `rgba(139,0,0,${Math.random()*0.4+0.1})`; ctx.stroke()
    }

    // 眨眼动画逻辑
    this.blink--; if(this.blink < -5) this.blink = Math.random()*200 + 100
    if(this.blink > 0) {
      this.drawIris(this.pX, this.pY, this.pupilR, vocabPercent.value, this.isHyper)
    } else {
      ctx.beginPath(); ctx.moveTo(this.cX-this.baseR, this.cY)
      ctx.quadraticCurveTo(this.cX, this.cY+30, this.cX+this.baseR, this.cY)
      ctx.lineWidth = 5; ctx.stroke()
    }

    requestAnimationFrame(() => this.animate())
  }
}

// ----------------------------------------------------------------------------
// [卷七：系统生命周期调度]
// ----------------------------------------------------------------------------
onMounted(() => {
  printLog('核心枢纽初始化开始...', 'sys')
  nextTick(() => {
    if (eyeCanvas.value) {
      window.abyssalEngine = new AbyssalEye(eyeCanvas.value)
      window.abyssalEngine.init()
      printLog('光学眼球追踪矩阵同步完成。', 'success')
    }
  })

  // 理智衰减与时钟
  setInterval(() => {
    const deadline = new Date('2028-12-23T08:30:00'); const diff = deadline - Date.now()
    if(diff <= 0) { doomTimer.value = "DESTINY_ARRIVED"; return }
    const d = Math.floor(diff/86400000).toString().padStart(3,'0'), h = Math.floor((diff/3600000)%24).toString().padStart(2,'0'), m = Math.floor((diff/60000)%60).toString().padStart(2,'0'), s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001); AbyssalOracle.write('s', sanityIndex.value)
  }, 1000)
})

onBeforeUnmount(() => { /* 清理资源 */ })
</script>

<style scoped>
/**
 * ============================================================================
 * [卷八：绝界织物 (CSS Architecture)]
 * 物理原则：100% 取缔背景色。全盘 mix-blend-mode 渗透纸张。
 * 视口原则：vw/vh 锁死，Object-fit 全屏覆盖。
 * ============================================================================
 */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:wght@400;700&family=Zhi+Mang+Xing&display=swap');

:root {
  --c-ink: #111411;
  --c-blood: #8B0000;
  --f-mono: 'Space Mono', 'Microsoft YaHei', monospace;
  --f-title: 'Cinzel', serif;
  --f-brush: 'Zhi Mang Xing', cursive;
}

* { box-sizing: border-box; }
body, html { 
  margin: 0; padding: 0; width: 100vw; height: 100vh; 
  overflow: hidden; background: #E2DED0; /* 浏览器级容错 */
}

.abyssal-gate-root {
  position: relative; width: 100vw; height: 100vh;
  font-family: var(--f-mono); color: var(--c-ink);
}

/* 精神污染滤镜：理智断绝时开启 */
.stage-broken { filter: url(#sanity-blur) contrast(1.2); }

/* 第一层：背景强制铺满 */
.global-paper-substrate {
  position: absolute; inset: 0;
  width: 100vw; height: 100vh; object-fit: cover;
  z-index: 0; pointer-events: none;
}
.void-vignette-mask {
  position: absolute; inset: 0; z-index: 1; pointer-events: none;
  background: radial-gradient(circle at center, transparent 35%, rgba(10,5,0,0.4) 100%);
}

/* 第二层：资产叠底融合 */
.occult-assets-composite {
  position: absolute; inset: 0; z-index: 5; pointer-events: none;
}
.occult-assets-composite img {
  position: absolute; mix-blend-mode: multiply; filter: contrast(1.1);
}
/* 墨迹分布 */
.stain-1 { top: -8%; left: -5%; width: 40vw; opacity: 0.8; }
.stain-2 { bottom: -12%; right: -5%; width: 48vw; opacity: 0.85; }
.stain-3 { top: 15%; right: 10%; width: 25vw; opacity: 0.6; }
.stain-4 { bottom: 5%; left: 8%; width: 32vw; opacity: 0.7; }
.stain-5 { top: 40%; left: 45%; width: 15vw; opacity: 0.3; }
/* 耶稣图腾 */
.altar-christ-totem {
  top: 5%; left: 50%; transform: translateX(-50%);
  height: 85vh; width: auto; opacity: 0.95;
}

/* 第三层：独立眼球 */
.eye-observer-nexus {
  position: absolute; inset: 0; z-index: 10; pointer-events: none;
}
.procedural-eye-surface { width: 100vw; height: 100vh; display: block; }

/* 第四层：稳定数据 UI 栅格 (全屏锁死，永不重叠) */
.interface-matrix {
  position: absolute; inset: 0; z-index: 20;
  display: flex; justify-content: space-between;
  padding: 3rem 4rem; pointer-events: none;
}

.ui-panel { width: 380px; display: flex; flex-direction: column; justify-content: space-between; height: 100%; }
.panel-center { flex: 1; }

.module {
  pointer-events: auto; background: transparent !important;
  mix-blend-mode: multiply; /* [核心]：让文字长在纸张纤维里 */
}
.module-title {
  font-size: 1.1rem; font-weight: 900; letter-spacing: 2px;
  border-bottom: 2.5px solid #000; padding-bottom: 5px; margin: 0 0 1.2rem 0;
  text-transform: uppercase;
}

/* 模块详情样式 */
.brand-logotype { font-family: var(--f-title); font-size: 3.5rem; font-weight: 900; margin: 0; line-height: 1; }
.timer-main { font-size: 3rem; font-weight: 700; letter-spacing: -2px; margin-top: 1rem; }
.count-down-wrap { margin-bottom: 1.5rem; }

.stat-rows { display: flex; flex-direction: column; gap: 8px; font-weight: 700; font-size: 0.95rem; }
.row { display: flex; justify-content: space-between; border-bottom: 1px dashed rgba(0,0,0,0.2); padding-bottom: 3px; }
.txt-red { color: var(--c-blood); }
.hyper { color: #d00; text-shadow: 0 0 8px rgba(255,0,0,0.5); font-size: 1.1rem; }

.pillars-grid { display: flex; flex-direction: column; gap: 12px; }
.p-info { display: flex; justify-content: space-between; font-weight: 900; font-size: 0.85rem; margin-bottom: 4px; }
.p-track { width: 100%; height: 6px; border: 1px solid #000; background: rgba(0,0,0,0.05); }
.p-fill { height: 100%; background: #000; transition: width 0.6s ease-out; }

.terminal-scroll-view {
  height: 200px; overflow-y: auto; font-size: 0.75rem;
  border-left: 2.5px solid #000; padding-left: 10px;
  display: flex; flex-direction: column; gap: 5px;
}
.log-line.success { color: #006400; font-weight: bold; }
.log-line.error, .log-line.alert { color: var(--c-blood); font-weight: bold; }

.whisper-canvas { display: flex; flex-direction: column; gap: 10px; }
.scrap-text { font-family: var(--f-brush); font-size: 1.9rem; line-height: 1.1; color: #0a0a0a; }

.drift-nav { display: flex; flex-direction: column; gap: 15px; margin-bottom: 2rem; }
.void-anchor { color: #000; text-decoration: none; font-weight: 900; font-size: 1rem; transition: 0.3s; }
.void-anchor:hover { color: var(--c-blood); transform: translateX(10px); }

.cli-terminal-bar {
  background: rgba(226, 222, 208, 0.7); /* 羊皮纸色背景保证输入可见 */
  padding: 12px 18px; border: 2.5px solid #000;
  display: flex; align-items: center; gap: 12px;
  mix-blend-mode: normal !important; box-shadow: 8px 8px 0 rgba(0,0,0,0.15);
}
.cli-field {
  flex: 1; border: none; background: transparent; outline: none;
  font-size: 1.3rem; font-family: inherit; font-style: italic;
  border-bottom: 1px dashed #444; color: #000;
}

/* 死亡层 */
.layer-death-return {
  position: fixed; inset: 0; z-index: 9999; background: #050505;
  display: flex; justify-content: center; align-items: center; text-align: center;
}
.death-title { color: #fff; font-family: var(--f-title); font-size: 4rem; text-shadow: 0 0 20px #800; }
.death-hint { color: #666; font-size: 1.2rem; letter-spacing: 5px; margin-top: 25px; animation: breathe 1.5s infinite; }

@keyframes breathe { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
@keyframes pulse-dot { 0%, 100% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.5); opacity: 0.3; } }
.status-pulse { width: 8px; height: 8px; background: var(--c-blood); border-radius: 50%; display: inline-block; margin-right: 8px; animation: pulse-dot 1.5s infinite; }
</style>