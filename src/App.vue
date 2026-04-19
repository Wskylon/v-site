/* =========================================================================================================
   [阿卡姆终极协议：V13.0 // 绝域观察者 THE ABSOLUTE OBSERVER (HUB)]
   [契约者：武少康 // 目标锚点：22408 科软飞升]
   [架构说明：
    1. 纯粹总控枢纽：剔除所有具体的考研长文本，代之以纯粹的物理因果粒子。
    2. 视口绝对锁死：利用 DPI 矩阵和 vw/vh 强制全屏，彻底解决缩放白边。
    3. 光学级纸墨融合：Canvas 底层重绘羊皮纸，所有素材通过 Multiply 算法物理消灭白底。
    4. 凝视核心：基于胡克定律(Hooke's law)的弹性物理眼球，瞳孔内嵌 53% 进度。
    5. 通灵接口中间件：RitualRegistry 完整实现，为 Node.js 数据库接入铺平道路。
   ]
   ========================================================================================================= */

<template>
  <div id="vzuor-hub-root" class="ascension-hub-nexus" :class="sanityStage">
    
    <svg width="0" height="0" class="occult-defs">
      <defs>
        <filter id="flesh-decay">
          <feGaussianBlur in="SourceGraphic" stdDeviation="1.2" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -7" result="goo" />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
        <filter id="void-tremor">
          <feTurbulence type="fractalNoise" baseFrequency="0.08" numOctaves="3" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="3" xChannelSelector="R" yChannelSelector="G" />
        </filter>
      </defs>
    </svg>

    <canvas ref="mainCanvas" class="omniscient-canvas-engine"></canvas>
    
    <div class="vignette-shadow"></div>

    <div class="embedded-ink-ui-layer">
      
      <header class="shard shard-header">
        <h1 class="artifact-title" data-text="V'ZUOR KHAA-SH'AN">V'ZUOR KHAA-SH'AN</h1>
        <div class="pulse-indicator">
          <span class="dot"></span> [协议 22408] 观察者枢纽运转正常
        </div>
        <div class="doom-timer">
          <span class="clock">{{ doomTimer }}</span>
          <p class="label">距星辰归位之时 (2028-12)</p>
        </div>
      </header>

      <nav class="shard shard-dimensions">
        <h2 class="shard-title">他维入口</h2>
        <div class="dimension-links">
          <a href="#" @click.prevent="jumpDim('web2')" class="void-link">【 维度二：思维孤岛 (知识典籍) 】</a>
          <a href="#" @click.prevent="jumpDim('web3')" class="void-link">【 维度三：因果回廊 (错题血肉) 】</a>
          <a href="#" @click.prevent="jumpDim('web4')" class="void-link">【 维度四：虚空裂隙 (模拟战场) 】</a>
        </div>
      </nav>

      <aside class="shard shard-pillars">
        <h2 class="shard-title">灵魂与枷锁监控</h2>
        <div class="bio-metrics">
          <div class="m-row"><span>理智残留 (SAN):</span> <span class="val text-blood">{{ sanityIndex.toFixed(4) }}%</span></div>
          <div class="m-row"><span>业障权重 (KARMA):</span> <span class="val">{{ karmaWeight.toFixed(2) }}</span></div>
          <div class="m-row"><span>灵药浓度:</span> <span class="val" :class="{'hyper': isHyper}">{{ caffeineLvl }} MG</span></div>
        </div>
        
        <div class="pillar-matrix">
          <div class="p-item">
            <div class="p-info"><span>Ⅰ 逻辑 (C语言 / DS)</span> <span>{{ progC }}%</span></div>
            <div class="p-track"><div class="p-fill" :style="{width: progC + '%'}"></div></div>
          </div>
          <div class="p-item">
            <div class="p-info"><span>Ⅱ 根基 (408 综合)</span> <span>{{ prog408 }}%</span></div>
            <div class="p-track"><div class="p-fill" :style="{width: prog408 + '%'}"></div></div>
          </div>
          <div class="p-item">
            <div class="p-info"><span>Ⅲ 虚空 (高阶数学)</span> <span>{{ progMath }}%</span></div>
            <div class="p-track"><div class="p-fill" :style="{width: progMath + '%'}"></div></div>
          </div>
          <div class="p-item">
            <div class="p-info"><span>Ⅳ 咒语 (考研英语)</span> <span>{{ progEng }}%</span></div>
            <div class="p-track"><div class="p-fill" :style="{width: progEng + '%'}"></div></div>
          </div>
        </div>
      </aside>

      <aside class="shard shard-logs">
        <h2 class="shard-title">阿卡夏记录</h2>
        <div class="terminal-box" ref="logBox">
          <div v-for="(l, i) in logs" :key="i" class="log-line" :class="l.type">
            <span class="ts">[{{ l.ts }}]</span> <span v-html="l.msg"></span>
          </div>
        </div>
      </aside>

      <aside class="shard shard-offering">
        <h2 class="shard-title">凡人供奉录 (留言板)</h2>
        <div class="offering-canvas">
          <div v-for="(msg, i) in messageBoard" :key="msg.id" class="whisper-text" :style="msg.style">
            " {{ msg.text }} "
          </div>
        </div>
      </aside>

      <footer class="shard shard-cli">
        <div class="cli-bar">
          <span class="prompt">root@vzuor:枢纽#</span>
          <input 
            v-model="cmdInput" 
            @keyup.enter="handleRitual"
            class="input-area" 
            placeholder="献祭进度 (vocab [数]) / 刻印留言 (msg [内容])..."
            spellcheck="false" autocomplete="off"
            :disabled="isDead"
          />
        </div>
      </footer>
    </div>

    <div class="death-shroud" v-if="isDead">
      <h1 class="death-title glitch">灵魂剥离 · 因果重塑</h1>
      <p class="death-sub">业障已锁死，正在重新载入世界线...</p>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ============================================================================
// [卷一：真名材质导入] 必须在 /src/assets/ 下
// ============================================================================
import imgPaper from './assets/paper.jpg'; import imgTotem from './assets/totem.jpg'
import imgInk1 from './assets/ink1.jpg'; import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg'; import imgInk4 from './assets/ink4.jpg'
import imgInk5 from './assets/ink5.jpg'
import imgTen1 from './assets/tentacle1.jpg'; import imgTen2 from './assets/tentacle2.jpg'

// ============================================================================
// [卷二：阿卡夏通灵中间件 (RitualRegistry)]
// 负责数据持久化，并预留完整的 RESTful 异步数据库对接逻辑，补足工程深度
// ============================================================================
class RitualRegistry {
  static get(key, def) { return Number(localStorage.getItem(`v13_${key}`)) || def }
  static set(key, val) { localStorage.setItem(`v13_${key}`, val) }
  
  static loadMessages() {
    try { return JSON.parse(localStorage.getItem('v13_msgs') || '[]') } 
    catch(e) { return [] }
  }
  
  static saveMessage(msgObj) {
    const msgs = this.loadMessages()
    msgs.push(msgObj)
    if(msgs.length > 15) msgs.shift() // 维持祭坛整洁，最多保留15条
    localStorage.setItem('v13_msgs', JSON.stringify(msgs))
    this.asyncSync('POST', '/api/v1/offerings', msgObj) // 触发后台同步
    return msgs
  }

  // [虚空钩子]：未来接入 Node.js/MongoDB 的异步引擎
  static async asyncSync(method, endpoint, payload) {
    console.log(`[数据库通灵口] 准备向 ${endpoint} 执行 ${method} 操作...`)
    /* try {
      const res = await fetch(`https://api.vzuor.com${endpoint}`, {
        method: method,
        headers: { 'Content-Type': 'application/json', 'Authorization': 'Bearer ASCENSION_TOKEN' },
        body: JSON.stringify(payload)
      });
      if (!res.ok) throw new Error('因果链断裂');
      console.log('数据库同步完成');
    } catch (err) {
      console.error('通灵失败:', err);
    }
    */
  }
}

// ============================================================================
// [卷三：状态机与响应式神经元]
// ============================================================================
const sanityIndex = ref(RitualRegistry.get('sanity', 99.9998))
const karmaWeight = ref(RitualRegistry.get('karma', 23.70))
const deathCount = ref(RitualRegistry.get('deaths', 0))
const caffeineLvl = ref(200)

// 核心进度：词汇量 (映射到 53% 瞳孔)
const vocabSiphoned = ref(RitualRegistry.get('vocab', 2150))
const vocabPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

// 四大柱神进度
const progC = ref(RitualRegistry.get('pc', 45))
const prog408 = ref(RitualRegistry.get('p408', 15))
const progMath = ref(RitualRegistry.get('pm', 20))
const progEng = ref(RitualRegistry.get('peng', 35))

const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref(''); const isHyper = ref(false); const isDead = ref(false)

// 日志与留言板
const logs = reactive([]); const logBox = ref(null)
const initialMsgs = RitualRegistry.loadMessages()
const messageBoard = reactive(initialMsgs.length ? initialMsgs : [
  { id: 1, text: "22408，唯有死战。", style: "top: 15%; left: 8%; transform: rotate(-4deg); opacity: 0.8;" },
  { id: 2, text: "计组的流水线压垮了我的理智...", style: "top: 45%; left: 12%; transform: rotate(3deg); opacity: 0.65;" }
])

const sanityStage = computed(() => sanityIndex.value > 80 ? 'san-safe' : (sanityIndex.value > 40 ? 'san-warn' : 'san-broken'))

// ============================================================================
// [卷四：系统原语指令]
// ============================================================================
const getTs = () => { const d = new Date(); return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}` }

const pushLog = (msg, type='info') => {
  logs.push({ ts: getTs(), msg, type })
  if(logs.length > 60) logs.shift()
  nextTick(() => { if(logBox.value) logBox.value.scrollTop = logBox.value.scrollHeight })
}

const jumpDim = (dim) => {
  pushLog(`尝试开启维度裂隙 [${dim}] ...`, 'sys')
  setTimeout(() => {
    pushLog(`维度跃迁被拒绝：该区域尚未在现实锚定。`, 'error')
    karmaWeight.value += 0.5; RitualRegistry.set('karma', karmaWeight.value)
  }, 600)
}

const handleRitual = () => {
  const raw = cmdInput.value.trim()
  if (!raw) return
  pushLog(`> ${raw}`, 'echo')
  const spaceIdx = raw.indexOf(' '); const cmd = spaceIdx > -1 ? raw.substring(0, spaceIdx).toLowerCase() : raw.toLowerCase()
  const arg = spaceIdx > -1 ? raw.substring(spaceIdx + 1).trim() : ''

  if (cmd === 'vocab') {
    const amt = parseInt(arg)
    if (amt > 0) {
      vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
      progEng.value = Math.min(100, progEng.value + (amt * 0.05))
      karmaWeight.value = Math.max(0, karmaWeight.value - amt*0.02)
      sanityIndex.value = Math.min(100, sanityIndex.value + 0.1)
      
      RitualRegistry.set('vocab', vocabSiphoned.value); RitualRegistry.set('peng', progEng.value)
      RitualRegistry.set('karma', karmaWeight.value); RitualRegistry.set('sanity', sanityIndex.value)
      RitualRegistry.asyncSync('PUT', '/api/v1/progress', { vocab: vocabSiphoned.value })
      
      pushLog(`吞噬灵魂。咒语柱神得到强化。当前同化率: ${vocabPercent.value}%`, 'success')
    } else pushLog('虚无的供品。', 'error')
  } 
  else if (cmd === 'dose') {
    const dose = parseInt(arg)
    if (dose > 0) {
      caffeineLvl.value = dose
      if (dose >= 400) {
        isHyper.value = true; if(window.vzuorSys) window.vzuorSys.hyper = true
        pushLog('警告：灵药致死量！物理法则正在剥离！', 'alert')
        setTimeout(() => { 
          isHyper.value = false; if(window.vzuorSys) window.vzuorSys.hyper = false
          caffeineLvl.value = 200; sanityIndex.value -= 5; RitualRegistry.set('sanity', sanityIndex.value)
          pushLog('反噬结束。机体功能重置。', 'sys')
        }, 7000)
      } else pushLog(`催化剂调整为 ${dose}MG。`, 'info')
    }
  }
  else if (cmd === 'msg') {
    if (arg) {
      // 随机手写样式生成器
      const rot = (Math.random() - 0.5) * 14
      const top = Math.random() * 70 + 5
      const left = Math.random() * 25 + 5
      const op = Math.random() * 0.4 + 0.6
      const newObj = {
        id: Date.now(), text: arg,
        style: `top: ${top}%; left: ${left}%; transform: rotate(${rot}deg); opacity: ${op};`
      }
      const updatedMsgs = RitualRegistry.saveMessage(newObj)
      messageBoard.splice(0, messageBoard.length, ...updatedMsgs)
      pushLog('供奉已刻印。', 'success')
    } else pushLog('不可言说的祈祷。', 'error')
  }
  else if (cmd === 'die') {
    isDead.value = true; deathCount.value++; RitualRegistry.set('deaths', deathCount.value)
    pushLog(`因果律强制切断。进入轮回周期：${deathCount.value}`, 'alert')
    setTimeout(() => {
      isDead.value = false; sanityIndex.value = 100; karmaWeight.value += 15
      RitualRegistry.set('sanity', 100); RitualRegistry.set('karma', karmaWeight.value)
      pushLog('世界线缝合完成。', 'sys')
    }, 4000)
  }
  else {
    karmaWeight.value += 1.2; RitualRegistry.set('karma', karmaWeight.value)
    pushLog('未知的低语。业障加深。', 'error')
  }
  cmdInput.value = ''
}

// ============================================================================
// [卷五：深渊光学引擎 (The Omniscient Physics Matrix)]
// 1. 视口绝对锁死 (DPI Scaling)
// 2. 纸墨重绘 (Canvas 彻底接管纸张，确保 multiply 完美运行)
// 3. 弹性追踪眼球 (Hooke's Law Physics)
// 4. 纯粹因果粒子飘落 (剔除考试文本)
// ============================================================================
const mainCanvas = ref(null)

const loadTex = (src) => new Promise(r => { const i = new Image(); i.src = src; i.onload = () => r(i) })

// [物理学：弹性眼球核心]
class HookeEye {
  constructor() {
    this.r = 85; this.pupilR = 38
    this.cx = 0; this.cy = 0 // 锚点 (由 resize 计算)
    this.px = 0; this.py = 0 // 瞳孔实际位置
    this.vx = 0; this.vy = 0 // 速度
    this.k = 0.08; this.friction = 0.82 // 弹簧系数与摩擦力 (制造果冻般的抖动感)
    this.blinkTimer = 150
  }
  update(mx, my) {
    // 1. 计算目标点 (受限于眼白内)
    const dx = mx - this.cx; const dy = my - this.cy
    const dist = Math.sqrt(dx*dx + dy*dy); const maxOff = this.r - this.pupilR - 8
    let targetX = this.cx, targetY = this.cy
    if (dist > 0) {
      targetX = this.cx + (dx/dist)*Math.min(dist*0.15, maxOff)
      targetY = this.cy + (dy/dist)*Math.min(dist*0.15, maxOff)
    }
    // 2. 弹簧物理模拟 (Hooke's Law)
    const ax = (targetX - this.px) * this.k; const ay = (targetY - this.py) * this.k
    this.vx = (this.vx + ax) * this.friction; this.vy = (this.vy + ay) * this.friction
    this.px += this.vx; this.py += this.vy

    this.blinkTimer--; if(this.blinkTimer < -5) this.blinkTimer = Math.random()*250 + 100
  }
  draw(ctx, pct, hyper) {
    ctx.save()
    // 眼白 (浑浊)
    ctx.beginPath(); ctx.arc(this.cx, this.cy, this.r, 0, Math.PI*2)
    ctx.fillStyle = 'rgba(230, 225, 215, 0.98)'; ctx.fill()
    ctx.lineWidth = 3; ctx.strokeStyle = '#050505'; ctx.stroke()
    
    // 恐怖血丝 (向心绘制)
    for(let i=0; i<40; i++) {
      const a = Math.random()*Math.PI*2
      const s = this.pupilR + Math.random()*10; const e = this.r - Math.random()*4
      ctx.beginPath(); ctx.moveTo(this.cx+Math.cos(a)*s, this.cy+Math.sin(a)*s)
      ctx.lineTo(this.cx+Math.cos(a)*e, this.cy+Math.sin(a)*e)
      ctx.strokeStyle = `rgba(150, 0, 0, ${Math.random()*0.4 + 0.1})`
      ctx.lineWidth = Math.random() > 0.8 ? 1.5 : 0.5; ctx.stroke()
    }

    if (this.blinkTimer > 0) {
      // 瞳孔
      ctx.beginPath(); ctx.arc(this.px, this.py, this.pupilR, 0, Math.PI*2)
      ctx.fillStyle = hyper ? 'rgba(120,0,0,0.95)' : '#0a0a0a'; ctx.fill()
      // 53% 蚀刻
      ctx.fillStyle = '#E2DED0'; ctx.font = "bold 34px Cinzel"; ctx.textAlign="center"; ctx.textBaseline="middle"
      ctx.fillText(`${pct}%`, this.px, this.py + 2)
      // 湿润反光
      ctx.beginPath(); ctx.arc(this.px - 14, this.py - 14, 5, 0, Math.PI*2)
      ctx.fillStyle = 'rgba(255,255,255,0.6)'; ctx.fill()
    } else {
      // 闭眼缝合线
      ctx.beginPath(); ctx.moveTo(this.cx - this.r, this.cy)
      ctx.bezierCurveTo(this.cx-20, this.cy+35, this.cx+20, this.cy+35, this.cx+this.r, this.cy)
      ctx.lineWidth = 5; ctx.strokeStyle = '#0a0a0a'; ctx.stroke()
    }
    ctx.restore()
  }
}

// [物理学：抽象因果粒子 (取缔长文本)]
class KarmaParticle {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = Math.random() * this.w; this.y = - (Math.random() * 800 + 50)
    this.vx = (Math.random() - 0.5) * 1.5; this.vy = Math.random() * 2 + 1
    this.r = Math.random() * 3 + 1; this.isBlood = Math.random() > 0.9
    // 抽象图形轨迹
    this.ang = Math.random()*Math.PI*2; this.spin = (Math.random()-0.5)*0.1
  }
  update(hyper) {
    this.x += Math.sin(this.ang)*0.5 + this.vx*(hyper?3:1)
    this.y += this.vy*(hyper?4:1); this.ang += this.spin
    if(this.y > this.h + 100) this.reset()
  }
  draw(ctx) {
    ctx.globalCompositeOperation = 'multiply'
    ctx.beginPath(); ctx.arc(this.x, this.y, this.r, 0, Math.PI*2)
    ctx.fillStyle = this.isBlood ? 'rgba(139,0,0,0.5)' : 'rgba(20,20,20,0.4)'
    ctx.fill()
  }
}

let sysState, tex = {}, pArray = []

const bootMatrix = async () => {
  const cvs = mainCanvas.value; ctx = cvs.getContext('2d')
  let w = 0, h = 0, dpr = 1
  
  // 视口强制物理锁死
  const lockViewport = () => {
    dpr = window.devicePixelRatio || 1
    w = window.innerWidth; h = window.innerHeight
    cvs.width = w * dpr; cvs.height = h * dpr
    cvs.style.width = `${w}px`; cvs.style.height = `${h}px`
    ctx.scale(dpr, dpr)
    
    if(sysState && sysState.eye) {
      sysState.eye.cx = w/2; sysState.eye.cy = h * 0.78 // 眼球固定在下半部
    }
  }
  lockViewport(); window.addEventListener('resize', lockViewport)

  pushLog('启动视口 DPI 矩阵锁死，加载底层材质...', 'sys')
  tex = {
    paper: await loadTex(imgPaper), totem: await loadTex(imgTotem),
    ink1: await loadTex(imgInk1), ink2: await loadTex(imgInk2),
    ten2: await loadTex(imgTen2)
  }
  pushLog('材质载入完毕。执行像素级 Multiply 光学融合。', 'success')

  sysState = { eye: new HookeEye(), mx: w/2, my: h/2, time: 0, hyper: false, id: null }
  window.vzuorSys = sysState
  window.addEventListener('mousemove', e => { sysState.mx = e.clientX; sysState.my = e.clientY })

  sysState.eye.cx = w/2; sysState.eye.cy = h*0.78; sysState.eye.px = w/2; sysState.eye.py = h*0.78
  for(let i=0; i<150; i++) pArray.push(new KarmaParticle(w, h))

  const render = () => {
    // 1. 基底重绘 (最完美解决白框的方案：底层用 Canvas 画羊皮纸)
    ctx.globalCompositeOperation = 'source-over'
    if(tex.paper) {
      // 类似 object-fit: cover 算法
      const ratio = Math.max(w/tex.paper.width, h/tex.paper.height)
      ctx.drawImage(tex.paper, 0, 0, tex.paper.width*ratio, tex.paper.height*ratio)
    } else {
      ctx.fillStyle = '#E2DED0'; ctx.fillRect(0,0,w,h)
    }

    // 2. 物理叠加层 (Multiply 烧录)
    ctx.globalCompositeOperation = 'multiply'
    pArray.forEach(p => { p.update(sysState.hyper); p.draw(ctx) }) // 坠落因果粒子
    
    // 墨迹与触手污染
    ctx.globalAlpha = sysState.hyper ? 0.9 : 0.75
    if(tex.ink1) ctx.drawImage(tex.ink1, -w*0.05, -h*0.1, w*0.4, h*0.5)
    if(tex.ink2) ctx.drawImage(tex.ink2, w*0.6, h*0.6, w*0.45, h*0.45)
    ctx.globalAlpha = sysState.hyper ? 0.35 : 0.2
    if(tex.ten2) ctx.drawImage(tex.ten2, w*0.05, h*0.1, w*0.2, h*0.3)

    // 核心祭坛 (完全融入羊皮纸，零白边)
    ctx.globalAlpha = 1.0
    if(tex.totem) {
      const th = h * 0.85; const tw = th * (tex.totem.width / tex.totem.height)
      const tx = w/2 - tw/2 + (sysState.hyper ? (Math.random()-0.5)*10 : 0)
      const ty = h*0.02 + (sysState.hyper ? (Math.random()-0.5)*10 : 0)
      ctx.drawImage(tex.totem, tx, ty, tw, th)
    }

    // 3. 实体层 (眼球)
    ctx.globalCompositeOperation = 'source-over'
    sysState.eye.update(sysState.mx, sysState.my)
    sysState.eye.draw(ctx, vocabPercent.value, sysState.hyper)

    sysState.time += sysState.hyper ? 0.08 : 0.02
    sysState.id = requestAnimationFrame(render)
  }
  render()
}

// ============================================================================
// [卷六：宿命时钟]
// ============================================================================
let clockId
onMounted(() => {
  pushLog('V13.0 观察者核心上线。神识已接通。', 'sys')
  nextTick(() => { bootMatrix() })
  
  clockId = setInterval(() => {
    const dObj = new Date('2028-12-23T08:30:00') - Date.now()
    if(dObj <= 0) { doomTimer.value = "DESTINY_ARRIVED"; clearInterval(clockId); return }
    const d = Math.floor(dObj/86400000).toString().padStart(3,'0')
    const h = Math.floor((dObj/3600000)%24).toString().padStart(2,'0')
    const m = Math.floor((dObj/60000)%60).toString().padStart(2,'0')
    const s = Math.floor((dObj/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.00008 - (deathCount.value*0.00002))
    RitualRegistry.set('sanity', sanityIndex.value)
  }, 1000)
})

onBeforeUnmount(() => { clearInterval(clockId); if(sysState?.id) cancelAnimationFrame(sysState.id) })

</script>

<style scoped>
/* ============================================================================
   [卷七：绝界光学织物 (CSS Architecture)]
   绝对原则：禁用 background-color。一切 UI 必须通过 multiply 化作墨水。
   视口原则：使用 vw/vh，杜绝滚动条，强制铺满。
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Zhi+Mang+Xing&display=swap');

:root {
  --c-ink: #0a0c0a; /* 极深墨色 */
  --c-blood: #8a0303;
  --c-muted: #4a4a4a;
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', 'Microsoft YaHei', monospace;
  --f-whisper: 'Zhi Mang Xing', cursive; /* 狂草 */
}

/* 视口强制锁死 */
body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: #E2DED0; }
.ascension-hub-nexus { position: relative; width: 100vw; height: 100vh; font-family: var(--f-mono); color: var(--c-ink); }

/* SVG滤镜池隐藏 */
.occult-defs { position: absolute; pointer-events: none; }
.san-warn { filter: contrast(1.1) sepia(0.15); }
.san-broken { filter: url(#flesh-decay) contrast(1.25); }

/* 底层DOM补丁 (Canvas 失败时的降级防御) */
.base-parchment-texture { position: absolute; inset: 0; width: 100vw; height: 100vh; object-fit: cover; z-index: 0; opacity: 0; /* 由Canvas负责绘制，此处仅作预留 */ }
.occult-noise-overlay { position: absolute; inset: 0; z-index: 1; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="4"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.04; }
.vignette-shadow { position: absolute; inset: 0; z-index: 2; pointer-events: none; background: radial-gradient(circle, transparent 40%, rgba(10,5,0,0.5) 100%); }

/* Canvas 核心层 */
.omniscient-canvas-engine { position: absolute; inset: 0; z-index: 10; display: block; width: 100vw; height: 100vh; }

/* ==============================================
   [无框墨迹 UI 矩阵 - 物理光学融合区]
   所有元素必须 mix-blend-mode: multiply，像墨迹一样印在底层 Canvas 上
   ============================================== */
.embedded-ink-ui-layer {
  position: relative; z-index: 100;
  width: 100vw; height: 100vh; padding: 2rem 3rem; box-sizing: border-box;
  pointer-events: none; /* 事件穿透给眼球追踪 */
}

/* 碎片基石：封杀 background 和 box-shadow */
.shard {
  position: absolute; pointer-events: auto;
  background: transparent !important; border: none !important; box-shadow: none !important; backdrop-filter: none !important;
  mix-blend-mode: multiply; /* 光学烧录核心 */
}
.shard-title { font-size: 1.1rem; letter-spacing: 2px; border-bottom: 2px solid var(--c-ink); padding-bottom: 4px; margin: 0 0 1rem 0; font-weight: 900; }

/* 天极 (左上) */
.shard-header { top: 2rem; left: 3rem; }
.artifact-title { font-family: var(--f-title); font-size: 3.5rem; font-weight: 900; margin: 0; position: relative; }
.artifact-title::before { content: attr(data-text); position: absolute; left: -2px; text-shadow: 2px 0 red; opacity: 0.4; animation: glitch 4s infinite linear alternate-reverse; }
.pulse-indicator { display: flex; align-items: center; gap: 8px; font-size: 0.8rem; font-weight: bold; color: var(--c-muted); margin-top: 5px; }
.dot { width: 6px; height: 6px; background: var(--c-blood); border-radius: 50%; animation: pulse 1.5s infinite; }
.doom-timer { margin-top: 15px; }
.clock { font-size: 2.8rem; font-weight: bold; letter-spacing: -2px; line-height: 1; }
.label { font-size: 0.75rem; font-weight: bold; color: var(--c-muted); margin-top: 5px; }

/* 多维入口 (左中) */
.shard-dimensions { top: 35%; left: 3rem; transform: translateY(-50%); width: 280px; }
.dimension-links { display: flex; flex-direction: column; gap: 15px; }
.void-link { color: var(--c-ink); text-decoration: none; font-weight: bold; font-size: 1.1rem; transition: 0.3s; position: relative; }
.void-link:hover { color: var(--c-blood); transform: translateX(10px) scale(1.02); text-shadow: 0 0 4px rgba(139,0,0,0.3); }
.void-link::before { content: ''; position: absolute; left: -12px; top: 50%; width: 5px; height: 5px; background: var(--c-blood); border-radius: 50%; opacity: 0; transition: 0.3s; }
.void-link:hover::before { opacity: 1; }

/* 灵魂与柱神监控 (左下) */
.shard-pillars { bottom: 2rem; left: 3rem; width: 340px; }
.bio-metrics { display: flex; flex-direction: column; gap: 8px; font-size: 0.95rem; font-weight: bold; margin-bottom: 1.5rem; }
.m-row { display: flex; justify-content: space-between; border-bottom: 1px dashed rgba(0,0,0,0.2); padding-bottom: 3px; }
.text-blood { color: var(--c-blood); }
.hyper { color: #d00; font-size: 1.1rem; text-shadow: 0 0 8px rgba(200,0,0,0.6); animation: jitter 0.1s infinite; }

.pillar-matrix { display: flex; flex-direction: column; gap: 10px; font-size: 0.85rem; font-weight: bold; }
.p-item { display: flex; flex-direction: column; gap: 4px; }
.p-info { display: flex; justify-content: space-between; }
.p-track { width: 100%; height: 5px; background: rgba(0,0,0,0.15); border: 1px solid var(--c-ink); }
.p-fill { height: 100%; background: var(--c-ink); transition: width 0.8s ease-out; }

/* 日志 (右上) */
.shard-logs { top: 2rem; right: 3rem; width: 360px; }
.terminal-box { height: 200px; overflow-y: auto; font-size: 0.75rem; display: flex; flex-direction: column; gap: 5px; padding-left: 10px; border-left: 2px solid var(--c-ink); }
.terminal-box::-webkit-scrollbar { width: 2px; }
.terminal-box::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.3); }
.log-line { line-height: 1.3; }
.ts { color: var(--c-muted); margin-right: 5px; }
.sys { color: #555; font-style: italic; }
.echo { color: #333; font-style: italic; opacity: 0.8; }
.success { color: #005500; font-weight: bold; text-shadow: 0 0 1px rgba(0,100,0,0.3); }
.error, .alert { color: var(--c-blood); font-weight: bold; }

/* 供奉录 (右中) */
.shard-offering { top: 52%; right: 3rem; transform: translateY(-50%); width: 400px; }
.offering-canvas { position: relative; height: 280px; width: 100%; }
.whisper-text { 
  position: absolute; 
  font-family: var(--f-whisper); /* 手写狂草质感 */
  font-size: 1.8rem; color: #050505; 
  line-height: 1.1; text-shadow: 0 0 2px rgba(0,0,0,0.2); 
  width: 90%; pointer-events: none;
}

/* 献祭台 (右下) - 唯一保留可见边框以供输入 */
.shard-cli { bottom: 2rem; right: 3rem; width: 480px; background: rgba(226, 222, 208, 0.6) !important; mix-blend-mode: normal !important; border: 2px solid var(--c-ink) !important; padding: 12px 20px; box-shadow: 6px 6px 0 rgba(0,0,0,0.1) !important; }
.cli-bar { display: flex; align-items: center; gap: 10px; }
.prompt { font-weight: 900; font-size: 1.1rem; color: #005500; }
.input-area { flex: 1; border: none; background: transparent; outline: none; font-size: 1.2rem; font-family: inherit; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed rgba(0,0,0,0.3); transition: 0.2s; }
.input-area:focus { border-bottom: 1px solid var(--c-ink); }
.input-area::placeholder { color: #777; font-size: 0.85rem; font-style: normal; }
.input-area:disabled { opacity: 0.3; }

/* 死亡层 */
.death-shroud { position: fixed; inset: 0; z-index: 9999; background: #050505; display: flex; justify-content: center; align-items: center; flex-direction: column; }
.death-title { color: #e2e2e2; font-family: var(--f-title); font-size: 4rem; letter-spacing: 8px; margin: 0; text-shadow: 0 0 15px #800; }
.death-sub { color: #666; font-size: 1.2rem; letter-spacing: 4px; margin-top: 20px; animation: pulse 1s infinite; }
.glitch { animation: violent-shake 0.1s infinite; }

/* 动画库 */
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes violent-shake { 0% { transform: translate(2px, 1px) rotate(0.5deg); } 50% { transform: translate(-1px, 2px) rotate(-0.5deg); } 100% { transform: translate(1px, -1px) rotate(0.5deg); } }
@keyframes pulse { 0%, 100% { opacity: 0.2; } 50% { opacity: 1; } }
@keyframes glitch { 0% { clip: rect(10px, 999px, 30px, 0); } 50% { clip: rect(60px, 999px, 80px, 0); } 100% { clip: rect(20px, 999px, 50px, 0); } }
</style>