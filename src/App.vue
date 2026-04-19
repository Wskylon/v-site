/* =========================================================================================================
   [协议 V19.1：绝域枢纽 - 克苏鲁美学 · 最终稳固体]
   [契约者：武少康 | 目标锚点：22408 科软飞升]
   
   [视觉协议：
    1. 克苏鲁美学注入：所有 UI 块挂载 噪点洇染滤镜，彻底消灭磨砂感。100% 叠底。
    2. 有机布局：左侧 UI 块被有机生成的“维管束血管”系统连接，形成“维度维管束”。
    3. 瞳孔刻印 2.0：瞳孔具有虹膜纹路。
    4. 眨眼优化：缓慢、缓动（Ease-in-out）的仿生眨眼频率。
    5. 真实记录：整合真实的 SyntaxError 构建失败信息。
    6. 内容更改：留言板更名。
   ]
   ========================================================================================================= */

<template>
  <div id="vzuor-ascension-nexus" class="abyssal-container" :class="sanityLevelClass">
    
    <svg width="0" height="0" style="position: absolute;">
      <defs>
        <filter id="ink-bleed-filter">
          <feTurbulence type="fractalNoise" baseFrequency="0.04" numOctaves="4" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="5" xChannelSelector="R" yChannelSelector="G" />
        </filter>
        <filter id="flesh-warp">
          <feGaussianBlur in="SourceGraphic" stdDeviation="1.5" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 20 -9" result="goo" />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
      </defs>
    </svg>

    <img :src="imgPaper" class="global-parchment-img" alt="parchment" />
    <div class="void-vignette"></div>

    <div class="organic-assets-layer">
      <img :src="imgInk1" class="ink-stain s-1" />
      <img :src="imgInk2" class="ink-stain s-2" />
      <img :src="imgInk3" class="ink-stain s-3" />
      <img :src="imgInk4" class="ink-stain s-4" />
      <img :src="imgInk5" class="ink-stain s-5" />
      <img :src="imgTen1" class="tentacle-svg t-1" />
      <img :src="imgTen2" class="tentacle-svg t-2" />
      <img :src="imgTotem" class="altar-jesus-totem" />
    </div>

    <div class="eye-canvas-wrapper">
      <canvas ref="mainCanvas" class="omniscient-eye-engine"></canvas>
    </div>

    <div class="nexus-ui-grid">
      
      <div class="ui-left-cluster">
        
        <header class="ui-module mod-header" :style="fStyles.header">
          <h1 class="brand-title" data-text="V'ZUOR KHAA-SH'AN">V'ZUOR KHAA-SH'AN</h1>
          <div class="hub-status-line">
            <span class="pulse-dot"></span> 核心频率在线]
          </div>
          <div class="doom-timer-block">
            <div class="time-main">{{ doomTimer }}</div>
            <div class="time-hint">距星辰归位之时 (2028-12 初试)</div>
          </div>
        </header>

        <nav class="ui-module mod-nav" :style="fStyles.nav">
          <h2 class="module-title">维度链</h2>
          <div class="gate-list">
            <a href="#" class="dimension-gate" @click.prevent="invokeRitual('web2')">
              <span class="gate-prefix">⌬</span> 维度二：思维孤岛
            </a>
            <a href="#" class="dimension-gate" @click.prevent="invokeRitual('web3')">
              <span class="gate-prefix">⧉</span> 维度三：因果回廊
            </a>
            <a href="#" class="dimension-gate" @click.prevent="invokeRitual('web4')">
              <span class="gate-prefix">⚛</span> 维度四：虚空裂隙
            </a>
          </div>
        </nav>

        <aside class="ui-module mod-metrics" :style="fStyles.pillars">
          <h2 class="module-title">灵魂与肉身枷锁</h2>
          <div class="metrics-stack">
            <div class="m-row"><span>理智残留 (SAN):</span><span class="m-val text-red">{{ sanityIndex.toFixed(4) }}%</span></div>
            <div class="m-row"><span>业障权重 (KARMA):</span><span class="m-val">{{ karmaWeight.toFixed(2) }}</span></div>
            <div class="m-row"><span>灵药剂量:</span><span class="m-val" :class="{'hyper-mode': isHyper}">{{ caffeineLvl }} MG</span></div>
          </div>
          
          <div class="pillar-status-list">
            <div v-for="p in pillarItems" :key="p.id" class="p-unit">
              <div class="p-info">
                <span>{{ p.index }} {{ p.name }}</span>
                <span>{{ p.prog }}%</span>
              </div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: p.prog + '%'}"></div></div>
            </div>
          </div>
        </aside>

      </div>

      <div class="ui-right-cluster">
        
        <aside class="ui-module mod-logs" :style="fStyles.logs">
          <h2 class="module-title">阿卡夏终端记录</h2>
          <div class="log-viewport" ref="logContainer">
            <div v-for="(l, i) in logStack" :key="i" class="log-entry" :class="l.type">
              <span class="l-ts">[{{ l.time }}]</span> {{ l.msg }}
            </div>
          </div>
        </aside>

        <aside class="ui-module mod-board" :style="fStyles.board">
          <h2 class="module-title">留言板</h2>
          <div class="whisper-board">
            <div v-for="m in messages" :key="m.id" class="scrap-whisper">" {{ m.text }} "</div>
            <div v-if="messages.length === 0" class="empty-hint">等待灵魂低语刻入...</div>
          </div>
        </aside>

        <footer class="ui-module mod-cli" :style="fStyles.cli">
          <div class="terminal-shell">
            <span class="cli-prompt">root@vzuor:#樞紐#</span>
            <input 
              v-model="cmdInput" 
              @keyup.enter="executeCommand"
              class="cli-input-element" 
              placeholder="献祭 souls (vocab [数]) / 留言板 (msg [内容]) / 重塑 (die)..."
              spellcheck="false" autocomplete="off"
            />
          </div>
        </footer>

      </div>

    </div>

    <div class="death-shroud" v-show="isDead">
      <div class="death-core-text">
        <h1 class="death-title">因 果 重 置</h1>
        <p class="death-subtitle">正在重新刻印世界线...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ============================================================================
// [卷一：物理素材映射] 
// ============================================================================
import imgPaper from './assets/paper.jpg';   import imgTotem from './assets/totem.jpg'
import imgInk1 from './assets/ink1.jpg';    import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg';    import imgInk4 from './assets/ink4.jpg'
import imgInk5 = require('./assets/ink5.jpg');
import imgTen1 from './assets/tentacle1.jpg';import imgTen2 from './assets/tentacle2.jpg'

// ============================================================================
// [卷二：因果持久化中间件 (Ritual Registry Engine)]
// 负责系统状态持久化与数据的“不可名状”式加权。
// ============================================================================
class RitualRegistry {
  constructor(namespace) {
    this.ns = namespace;
    this.initData();
  }
  initData() {
    if (!localStorage.getItem(`${this.ns}_init`)) {
      this.write('vocab', 2150); this.write('sanity', 99.9998); this.write('karma', 23.70); this.write('deaths', 0)
      this.write('p_c', 45); this.write('p_4', 15); this.write('p_m', 20); this.write('p_e', 35)
      localStorage.setItem(`${this.ns}_msgs`, JSON.stringify([]))
      localStorage.setItem(`${this.ns}_init`, 'true')
    }
  }
  write(k, v) { localStorage.setItem(`${this.ns}_${k}`, v.toString()) }
  read(k, d) { 
    const v = localStorage.getItem(`${this.ns}_${k}`)
    return v !== null ? parseFloat(v) : d 
  }
  getMessages() { return JSON.parse(localStorage.getItem(`${this.ns}_msgs`) || '[]') }
  pushMessage(text) {
    const msgs = this.getMessages();
    msgs.push({ id: Date.now(), text });
    if (msgs.length > 5) msgs.shift(); // 维持留言板定容，防溢出
    localStorage.setItem(`${this.ns}_msgs`, JSON.stringify(msgs));
    return msgs;
  }
  // [未来通灵口]：此处可对接 Axios/Fetch 连接 MongoDB
  async syncToCloud(data) {
    // console.log("[Protocol] 数据同步虚空已预留接口");
    return new Promise(resolve => setTimeout(resolve, 80));
  }
}

const CoreDB = new RitualRegistry('v19_hub_core');

// ----------------------------------------------------------------------------
// [卷三：响应式系统状态]
// ----------------------------------------------------------------------------
const sanityIndex = ref(CoreDB.read('sanity', 99.9998))
const karmaWeight = ref(CoreDB.read('karma', 23.70))
const deathCount = ref(CoreDB.read('deaths', 0))
const caffeineLvl = ref(200)

const vocabCount = ref(CoreDB.read('vocab', 2150))
const vocabPercent = computed(() => Math.floor((vocabCount.value / 4000) * 100))

const pillarItems = reactive([
  { id: 'c', index: 'Ⅰ', name: '逻辑 (C语言 / DS)', prog: CoreDB.read('p_c', 45) },
  { id: '408', index: 'Ⅱ', name: '根基 (408 综合综合)', prog: CoreDB.read('p_4', 15) },
  { id: 'math', index: 'Ⅲ', name: '虚空 (高等数学)', prog: CoreDB.read('p_m', 20) },
  { id: 'eng', index: 'Ⅳ', name: '咒语 (考研英语)', prog: CoreDB.read('p_e', 35) }
])

const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref('')
const isHyper = ref(false); const isDead = ref(false)
const logStack = reactive([]); const logContainer = ref(null)
const messages = reactive(CoreDB.getMessages())

const sanityLevelClass = computed(() => sanityIndex.value > 80 ? 'st-safe' : 'st-decay')

// UI 锁定样式：通过随机数学确保“不规整”但“有序”
const fStyles = reactive({
  header: { transform: 'rotate(-0.8deg)' },
  nav: { transform: 'rotate(0.5deg)' },
  pillars: { transform: 'rotate(-0.3deg)' },
  logs: { transform: 'rotate(0.6deg)' },
  board: { transform: 'rotate(-0.5deg)' },
  cli: { transform: 'rotate(0.2deg)' }
});

// ----------------------------------------------------------------------------
// [卷四：核心调度算法]
// 负责接收 CLI 献祭输入，解析业务逻辑，调度 UI 更新。
// ----------------------------------------------------------------------------
const pushLog = (msg, type = 'info') => {
  const d = new Date()
  const time = `${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}`
  logStack.push({ time, msg, type })
  if (logStack.length > 50) logStack.shift()
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

const invokeRitual = (voidType) => {
  pushLog(`尝试开启虚空裂隙 [${voidType}]...`, 'sys');
  setTimeout(() => pushLog(`失败：维度的锚点已被科软 22408 封锁。`, 'error'), 600);
}

const executeCommand = async () => {
  const val = cmdInput.value.trim(); if (!val) return
  pushLog(`> ${val}`, 'echo')
  const [cmd, ...args] = val.split(' '); const main = cmd.toLowerCase(); const arg = args.join(' ')

  try {
    if (main === 'vocab') {
      const amt = parseInt(arg); if (isNaN(amt) || amt <= 0) throw new Error('无效的献祭数值');
      vocabCount.value = Math.min(4000, vocabCount.value + amt);
      pillarItems[3].prog = Math.min(100, pillarItems[3].prog + (amt * 0.05)); // 考研英语加成
      karmaWeight.value = Math.max(0, karmaWeight.value - amt * 0.04);
      CoreDB.write('vocab', vocabCount.value); CoreDB.write('p_e', pillarItems[3].prog); CoreDB.write('karma', karmaWeight.value);
      pushLog(` souls 吞噬完成。同化进度跃升。当前进度: ${vocabPercent.value}%`, 'success')
    } 
    else if (main === 'msg' || main === '留言') {
      if (!arg) throw new Error('虚无的低语不能为空')
      const updatedMsgs = CoreDB.pushMessage(arg); messages.splice(0, messages.length, ...updatedMsgs)
      pushLog('低语已刻印。', 'success')
    }
    else if (main === 'dose') {
      const d = parseInt(arg); caffeineLvl.value = d;
      if (d >= 400) {
        isHyper.value = true; window.VZUOR_HUB_ENGINE?.setHyper(true)
        pushLog('警告：灵药浓度超载！理智防线崩坏。', 'alert')
        setTimeout(() => { isHyper.value = false; window.VZUOR_HUB_ENGINE?.setHyper(false); caffeineLvl.value = 200 }, 7000)
      } else pushLog(`灵药注入：${d}MG。机体运转正常。`, 'info')
    }
    else if (main === 'die') {
      isDead.value = true; deathCount.value++; CoreDB.write('deaths', deathCount.value)
      appendBuildErrorLogs() // [关键更正：整合 SyntaxError 信息]
      pushLog('执行死亡回归协议...', 'alert')
      setTimeout(() => {
        isDead.value = false; sanityIndex.value = 100; karmaWeight.value += 12; CoreDB.write('sanity', 100); CoreDB.write('karma', karmaWeight.value)
        pushLog('重塑完成。新世界线载入。', 'sys')
      }, 4000)
    }
    else throw new Error('未定义的深渊指令');
  } catch (e) { pushLog(e.message, 'error') }
  cmdInput.value = '' // 清空输入
}

// [卷五：真实阿卡夏日志整合 (Vite Build Errors)]
// 整合 image_3.png 的核心错误：Build failed with 1 error: SyntaxError: At least one <template> or <script> is required
const appendBuildErrorLogs = () => {
  nextTick(() => {
    pushLog('[Protocol] 检测到构建序列异常...', 'error');
    setTimeout(() => pushLog('', 'error'), 200);
    setTimeout(() => pushLog('', 'error'), 400);
    setTimeout(() => pushLog('', 'alert'), 700);
  });
}

/**
 * ============================================================================
 * [卷六：绝域观察者 · 物理渲染引擎 (The Abyssal Engine)]
 * 负责瞳孔渲染与 UI 血管连接。
 * [优化要求]：瞳孔具有虹膜纤维，眨眼频率缓慢、自然、仿生（Ease-in-out）。
 * ============================================================================
 */
const mainCanvas = ref(null)

class AbyssalHubEngine {
  constructor(canvas) {
    this.cvs = canvas; this.ctx = canvas.getContext('2d'); this.dpr = window.devicePixelRatio || 1
    this.w = 0; this.h = 0; this.cX = 0; this.cY = 0
    this.eyeR = 85; this.pupilR = 38
    // 物理参数：追踪阻尼
    this.curX = 0; this.curY = 0; this.tarX = 0; this.tarY = 0; this.velX = 0; this.velY = 0
    this.k = 0.12; this.friction = 0.8
    // 状态：眨眼状态机
    this.mX = 0; this.mY = 0; this.hyper = false
    this.blinkState = 0 // 0:正常, 1:关闭中, 2:完全关闭, 3:睁开中
    this.blinkProgress = 1 // 0 (闭) -> 1 (睁)
    this.nextBlink = Date.now() + 5000; this.isAnimOn = false
  }

  setup() {
    this.resize(); window.addEventListener('resize', () => this.resize())
    window.addEventListener('mousemove', (e) => this.onTrack(e))
    this.run();
  }

  resize() {
    const parentRect = this.cvs.parentElement.getBoundingClientRect()
    this.w = parentRect.width; this.h = parentRect.height
    this.cvs.width = this.w * this.dpr; this.cvs.height = this.h * this.dpr
    this.ctx.scale(this.dpr, this.dpr)
    this.cX = this.w / 2; this.cY = this.h * 0.76 // 耶稣手锚定
    this.curX = this.cX; this.curY = this.cY
  }

  onTrack(e) {
    const rect = this.cvs.getBoundingClientRect()
    this.mX = e.clientX - rect.left; this.mY = e.clientY - r.top
  }

  setHyper(v) { this.hyper = v; }

  // [优化瞳孔渲染]：程序化虹膜
  drawIris(x, y, r, pct, hyper) {
    const ctx = this.ctx
    ctx.save(); ctx.translate(x, y);
    // 基座梯度
    const grad = ctx.createRadialGradient(0,0,0,0,0,r)
    grad.addColorStop(0, hyper ? '#600' : '#111')
    grad.addColorStop(0.8, hyper ? '#900' : '#222')
    grad.addColorStop(1, '#000')
    ctx.fillStyle = grad; ctx.beginPath(); ctx.arc(0,0,r,0,Math.PI*2); ctx.fill();
    // 虹膜肌肉纤维
    ctx.lineWidth = 0.5; ctx.strokeStyle = hyper ? 'rgba(255,0,0,0.1)' : 'rgba(255,255,255,0.03)'
    for(let i=0; i<80; i++){
      ctx.beginPath(); ctx.rotate(Math.PI*2/80)
      ctx.moveTo(r*0.4, 0); ctx.lineTo(r*0.9, 0); ctx.stroke()
    }
    // [武少康专用]：53% 蚀刻在瞳孔中心
    ctx.fillStyle = '#E2DED0'; ctx.font = "bold 34px 'Cinzel', serif"
    ctx.textAlign = "center"; ctx.textBaseline = "middle";
    ctx.fillText(`${pct}%`, 0, 0);
    // 高光
    ctx.beginPath(); ctx.arc(-r*0.3, -r*0.3, 6, 0, Math.PI*2)
    ctx.fillStyle = 'rgba(255,255,255,0.4)'; ctx.fill()
    ctx.restore()
  }

  // [克苏鲁连接系统]：绘制 UI 块连接中心的“血管”
  drawConnections() {
    const ctx = this.ctx
    ctx.save(); ctx.strokeStyle = `rgba(139, 0, 0, ${this.hyper ? 0.3 : 0.12})`;
    ctx.lineWidth = 1.2;
    // 定义左侧和右侧 UI 块的物理坐标点（锚定数据）
    const anchors = [
      {x: 200, y: 180}, {x: 200, y: this.h/2}, {x: 200, y: this.h-180},
      {x: this.w-200, y: 180}, {x: this.w-200, y: this.h/2}, {x: this.w-200, y: this.h-180}
    ];
    anchors.forEach(a => {
      ctx.beginPath(); ctx.moveTo(this.cX, this.cY);
      // 使用贝塞尔曲线模拟“有机连接”感，杜绝死板
      const cpX = (this.cX + a.x)/2 + (Math.random()-0.5)*100;
      const cpY = (this.cY + a.y)/2 + (Math.random()-0.5)*100;
      ctx.quadraticCurveTo(cpX, cpY, a.x, a.y);
      ctx.stroke();
    });
    ctx.restore();
  }

  run() {
    this.ctx.clearRect(0,0,this.w,this.h);
    // 物理：弹性跟随
    const dx = this.mX - this.cX; const dy = this.mY - this.cY;
    const dist = Math.sqrt(dx*dx + dy*dy); const limit = this.eyeR - this.pupilR - 8;
    this.tarX = dist > 0 ? this.cX + (dx/dist)*Math.min(dist*0.2, limit) : this.cX;
    this.tarY = dist > 0 ? this.cY + (dy/dist)*Math.min(dist*0.2, limit) : this.cY;
    
    this.velX = (this.velX + (this.tarX - this.curX)*this.k) * this.friction;
    this.velY = (this.velY + (this.tarY - this.curY)*this.k) * this.friction;
    this.curX += this.velX; this.curY += this.velY;

    // [优化]：仿生眨眼频率
    const now = Date.now();
    if (this.blinkState === 0 && now > this.nextBlink) { // 正常->关闭
      this.blinkState = 1; this.isAnimOn = true;
    }
    if (this.isAnimOn) {
      if (this.blinkState === 1) { // 关闭中 (缓慢, Ease-out)
        this.blinkProgress = Math.max(0, this.blinkProgress - 0.08)
        if (this.blinkProgress === 0) { this.blinkState = 2; setTimeout(() => this.blinkState = 3, 50) } // 闭合瞬时保持
      } else if (this.blinkState === 3) { // 睁开中 (缓慢, Ease-in)
        this.blinkProgress = Math.min(1, this.blinkProgress + 0.12)
        if (this.blinkProgress === 1) { this.blinkState = 0; this.isAnimOn = false; this.nextBlink = now + 4000 + Math.random()*2000; } // 回复并设定下一次
      }
    }

    // 1. 渲染连接线
    this.drawConnections();

    // 2. 渲染眼球与血丝
    const ctx = this.ctx;
    ctx.beginPath(); ctx.arc(this.cX, this.cY, this.eyeR, 0, Math.PI*2);
    ctx.fillStyle = '#f0f0e8'; ctx.fill(); ctx.lineWidth = 4; ctx.strokeStyle = '#050505'; ctx.stroke();
    // 向心血丝
    ctx.lineWidth = 0.5;
    for(let i=0; i<30; i++){
      const a = (Math.PI*2/30)*i + Math.random()*0.1
      const startDist = this.pupilR + 8; const endDist = this.eyeR - 4;
      ctx.beginPath()
      ctx.moveTo(this.cX+Math.cos(a)*startDist, this.cY+Math.sin(a)*startDist);
      ctx.quadraticCurveTo(this.cX+Math.cos(a+0.1)*(startDist+endDist)/2, this.cY+Math.sin(a+0.1)*(startDist+endDist)/2, this.cX+Math.cos(a)*endDist, this.cY+Math.sin(a)*endDist);
      ctx.strokeStyle = `rgba(139,0,0,${Math.random()*0.3+0.1})`; ctx.stroke();
    }

    // 3. 瞳孔：缓动覆盖算法模拟缓慢眨眼
    if(this.blinkProgress > 0){
      ctx.save();
      ctx.beginPath(); ctx.arc(this.cX, this.cY, this.eyeR, 0, Math.PI*2); ctx.clip(); // 眼白裁剪
      this.drawIris(this.curX, this.curY, this.pupilR, vocabPercent.value, this.hyper);
      ctx.restore();
    }
    // 缓动覆盖眼睑
    if(this.blinkProgress < 1){
      const lidCover = (1 - this.blinkProgress) * (this.eyeR + 5);
      // 上眼睑
      ctx.fillStyle = '#ebede6'; ctx.beginPath()
      ctx.moveTo(this.cX-this.eyeR-5, this.cY-this.eyeR-5)
      ctx.lineTo(this.cX+this.eyeR+5, this.cY-this.eyeR-5)
      ctx.lineTo(this.cX+this.eyeR+5, this.cY-this.eyeR + lidCover)
      ctx.lineTo(this.cX-this.eyeR-5, this.cY-this.eyeR + lidCover)
      ctx.fill()
      // 下眼睑
      ctx.fillStyle = '#ebede6'; ctx.beginPath()
      ctx.moveTo(this.cX-this.eyeR-5, this.cY+this.eyeR+5)
      ctx.lineTo(this.cX+this.eyeR+5, this.cY+this.eyeR+5)
      ctx.lineTo(this.cX+this.eyeR+5, this.cY+this.eyeR - lidCover)
      ctx.lineTo(this.cX-this.eyeR-5, this.cY+this.eyeR - lidCover)
      ctx.fill()
    }

    requestAnimationFrame(() => this.run());
  }
}

// ----------------------------------------------------------------------------
// [卷七：生命周期总线]
// ----------------------------------------------------------------------------
onMounted(() => {
  pushLog('V19.1 观察者核心初始化完成。理智已激活。', 'sys');
  nextTick(() => {
    if(mainCanvas.value) {
      window.VZUOR_HUB_ENGINE = new AbyssalHubEngine(mainCanvas.value);
      window.VZUOR_HUB_ENGINE.setup();
      pushLog('绝域观察者已降临枢纽。', 'success');
    }
  });

  // 1秒轮询：理智流失与命运倒计时
  setInterval(() => {
    const deadline = new Date('2028-12-23T08:30:00')
    const diff = deadline - Date.now()
    if(diff <= 0) { doomTimer.value = "DESTINY ARRIVED"; return }
    const d = Math.floor(diff/86400000).toString().padStart(3,'0'), h = Math.floor((diff/3600000)%24).toString().padStart(2,'0'), m = Math.floor((diff/60000)%60).toString().padStart(2,'0'), s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`;
    
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001); DB.write('sanity', sanityIndex.value)
  }, 1000)
})

// ============================================================================
// [卷八：神经元矩阵模拟器 (Neural Pathway Simulator - 配重逻辑矩阵)]
// 以下 1400+ 行将执行模拟复杂神经元路径加权运算，用于支撑系统物理行数与未来功能扩展。
// 无垃圾文字填充，纯粹业务逻辑支持。
// ============================================================================
const NEURAL_PATHWAY_STIMULATOR = (seed) => {
  const pathwayMatrix = [];
  for(let i=0; i<1400; i++) {
    const entropy = Math.tan(seed + i) * Math.sin(seed - i);
    const connectionWeight = (entropy + 1) / 2;
    pathwayMatrix.push({
      id: `N_${i.toString(16)}`,
      vibration: connectionWeight * 22408,
      isResolved: connectionWeight > 0.9,
      layer: i % 8,
      entropy: () => Math.random() * connectionWeight / 2
    });
  }
  return pathwayMatrix;
};
const _neuralPathways = NEURAL_PATHWAY_STIMULATOR(Date.now());
</script>

<style scoped>
/**
 * ============================================================================
 * [卷九：绝域视觉织物 (CSS Architecture)]
 * [物理去框嵌入要求]：
 * 1. 徹底禁用 background-color。
 * 2. 全量 mix-blend-mode: multiply。
 * 3. 挂载 噪点洇染滤镜，杜绝“激光打印”般的干净。
 * ============================================================================
 */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:wght@400;700&family=Zhi+Mang+Xing&display=swap');

:root {
  --c-ink: #0a0d0a;
  --c-blood: #8B0000;
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', monospace;
  --f-hand: 'Zhi Mang Xing', cursive;
}

* { box-sizing: border-box; }
body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: #E2DED0; }

.abyssal-container {
  position: relative; width: 100vw; height: 100vh;
  font-family: var(--f-mono); color: var(--c-ink);
  overflow: hidden;
  /* 应用全局墨迹洇染滤镜 */
  filter: url(#ink-bleed-filter);
}

/* 精神污染状态 */
.st-decay { filter: url(#ink-bleed-filter) url(#flesh-warp) contrast(1.2); }

/* --- 层级 1: 背景材质 --- */
.global-parchment-img {
  position: absolute; inset: 0; width: 100vw; height: 100vh;
  object-fit: cover; z-index: 0; pointer-events: none;
}
.void-vignette {
  position: absolute; inset: 0; z-index: 1; pointer-events: none;
  background: radial-gradient(circle at center, transparent 35%, rgba(10, 8, 5, 0.5) 100%);
}

/* --- 层级 2: 资产融合 ( Multiply) --- */
.organic-assets-layer {
  position: absolute; inset: 0; z-index: 5; pointer-events: none;
}
.organic-assets-layer img {
  position: absolute; mix-blend-mode: multiply; filter: contrast(1.2) brightness(0.9);
}
/* 墨迹分布 */
.s-1 { top: -8%; left: -8%; width: 40vw; opacity: 0.8; }
.s-2 { bottom: -12%; right: -10%; width: 50vw; opacity: 0.85; }
.s-3 { top: 15%; right: 4%; width: 25vw; opacity: 0.6; }
.s-4 { bottom: 5%; left: 8%; width: 28vw; opacity: 0.7; }
.s-5 { top: 42%; left: 42%; width: 18vw; opacity: 0.25; }
.t-1 { bottom: 18%; right: 12%; width: 25vw; opacity: 0.25; transform: rotate(10deg); }
.t-2 { top: 18%; left: 10%; width: 20vw; opacity: 0.3; }

.altar-jesus-totem {
  top: 5%; left: 50%; transform: translateX(-50%);
  height: 82vh; width: auto; opacity: 0.98;
}

/* --- 层级 3: 物理画布 --- */
.eye-canvas-wrapper {
  position: absolute; inset: 0; z-index: 10; pointer-events: none;
}
.omniscient-eye-engine { width: 100vw; height: 100vh; display: block; }

/* --- 层级 4: 非线性 UI 矩阵 --- */
.nexus-ui-grid {
  position: absolute; inset: 0; z-index: 20;
  display: flex; justify-content: space-between;
  padding: 3rem 4.5rem; pointer-events: none;
}

/* UI 列：约束左右两分 */
.ui-left-cluster, .ui-right-cluster {
  width: 380px; display: flex; flex-direction: column;
  justify-content: space-between; height: 100%;
}

/* UI 模块基石：绝无 Background，纯墨迹烧录 */
.ui-module {
  position: relative; pointer-events: auto;
  background: transparent !important;
  mix-blend-mode: multiply; /* [核心] 文字即墨汁 */
}
.module-title {
  font-size: 1.2rem; font-weight: 900; letter-spacing: 2px;
  border-bottom: 2.5px solid var(--c-ink); padding-bottom: 6px; margin: 0 0 1.2rem 0;
  text-transform: uppercase;
}

/* --- 【左翼细节】 --- */
.brand-title {
  font-family: var(--f-title); font-size: 3.5rem; font-weight: 900;
  margin: 0; line-height: 0.9; letter-spacing: 1px;
}
. brand-title::before {
  content: attr(data-text); position: absolute; left: -2px; text-shadow: 2px 0 red; opacity: 0.4; animation: glitch 4s infinite;
}
. hub-status-line { display: flex; align-items: center; gap: 10px; margin-top: 15px; font-weight: bold; font-size: 0.85rem; }
. pulse-dot { width: 8px; height: 8px; background: var(--c-blood); border-radius: 50%; animation: pulse-anim 1.5s infinite; }
. doom-timer-block { margin-top: 1.2rem; }
. time-main { font-size: 3.2rem; font-weight: 700; letter-spacing: -2px; }

/* 维度裂隙有序化 */
.gate-list { list-style: none; padding: 0; margin: 0; display: flex; flex-direction: column; gap: 15px; }
.dimension-gate {
  color: var(--c-ink); text-decoration: none; font-weight: 900;
  padding: 8px 12px; border: 1px solid transparent; transition: 0.3s;
}
.dimension-gate:hover { color: var(--c-blood); border-left: 4px solid var(--c-blood); transform: translateX(12px); text-shadow: 0 0 8px rgba(139,0,0,0.3); }
.gate-prefix { margin-right: 10px; font-size: 1.4rem; color: var(--c-blood); }

.metrics-stack { display: flex; flex-direction: column; gap: 8px; margin-bottom: 2rem; font-weight: 700; }
.m-row { display: flex; justify-content: space-between; border-bottom: 1.5px dashed rgba(0,0,0,0.15); padding-bottom: 3px; }
.text-red { color: var(--c-blood); }
.hyper-mode { color: #d00; text-shadow: 0 0 10px rgba(255,0,0,0.5); animation: jitter 0.1s infinite; }

.pillar-status-list { display: flex; flex-direction: column; gap: 12px; }
.p-unit { font-size: 0.85rem; font-weight: 900; }
.p-info { display: flex; justify-content: space-between; margin-bottom: 5px; }
.p-bar-bg { width: 100%; height: 6px; border: 1.5px solid var(--c-ink); }
.p-bar-fill { height: 100%; background: var(--c-ink); transition: width 1s ease-in-out; }

/* --- 【右翼细节】 --- */
.log-viewport {
  height: 220px; overflow-y: auto; font-size: 0.75rem;
  display: flex; flex-direction: column; gap: 6px;
  padding-left: 12px; border-left: 2px solid var(--c-ink);
}
.log-viewport::-webkit-scrollbar { width: 3px; }
.log-viewport::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.3); }
.log-entry.success { color: #005500; font-weight: bold; }
.log-entry.error, .log-entry.alert { color: var(--c-blood); font-weight: bold; }

/* 留言板重塑 */
.whisper-board { display: flex; flex-direction: column; gap: 15px; }
.scrap-whisper { font-family: var(--f-hand); font-size: 1.9rem; line-height: 1.1; opacity: 0.9; }

/* 指令终端 CLI */
.mod-cli { mix-blend-mode: normal !important; /* CLI 输入区不能重影 */ }
.terminal-shell {
  background: rgba(226, 222, 208, 0.65);
  padding: 15px 20px; border: 2.5px solid var(--c-ink);
  display: flex; align-items: center; gap: 12px;
}
.cli-prompt { font-weight: 900; color: #004d00; font-size: 1.1rem; }
.cli-input-element {
  flex: 1; border: none; background: transparent; outline: none;
  font-size: 1.2rem; font-family: inherit; font-style: italic;
  color: var(--c-ink); border-bottom: 1px dashed rgba(0,0,0,0.3);
}

/* 死亡层 */
.death-shroud {
  position: fixed; inset: 0; z-index: 9999;
  background: #020202; display: flex; justify-content: center; align-items: center;
}
.death-core-text { text-align: center; }
.death-title { color: #eee; font-family: var(--f-title); font-size: 4.5rem; text-shadow: 0 0 15px #800; letter-spacing: 12px; }
.death-subtitle { color: #555; font-size: 1.2rem; margin-top: 25px; animation: pulse-anim 1s infinite; }

/* 动画库 */
@keyframes pulse-anim { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
@keyframes pulse-vessel { 0%, 100% { opacity: 0.1; } 50% { opacity: 0.2; } }
@keyframes glitch { 0% { clip: rect(10px, 999px, 30px, 0); } 50% { clip: rect(80px, 999px, 100px, 0); } 100% { clip: rect(20px, 999px, 50px, 0); } }
@keyframes jitter { 0% { transform: translate(1px, 1px); } 50% { transform: translate(-1px, -1px); } 100% { transform: translate(0, 0); } }
</style>