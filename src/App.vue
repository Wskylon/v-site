/* =========================================================================================================
   [协议 V14.0：绝对深渊枢纽 (ABSOLUTE ABYSSAL HUB)]
   [契约者：武少康 | 目标：22408 科软]
   [执行动作：
    1. 物理层接管：利用原生 DOM <img> 强制加载 9 张本地素材，100% 杜绝白屏。
    2. 光学混合协议：强制注入 mix-blend-mode: multiply，彻底湮灭所有 JPG 白底。
    3. UI 锚定协议：启用绝对稳定的 Flex/Grid 布局，取缔所有乱飞的 Absolute 定位。
    4. 仿生眼球引擎：剥离出独立的局部 Canvas，仅运行眼球的视网膜生成与视线追踪算法。
    5. 中间件系统：构建企业级 RitualDB 中间件，模拟全量后端 CRUD 交互。
   ]
   ========================================================================================================= */

<template>
  <div id="ascension-hub-v14" class="hub-container">
    
    <img :src="imgPaper" class="layer-base-parchment" alt="Paper Background" />
    <div class="layer-global-vignette"></div>

    <div class="layer-art-assets">
      <img :src="imgInk1" class="asset-ink pos-ink-1" alt="ink1" />
      <img :src="imgInk2" class="asset-ink pos-ink-2" alt="ink2" />
      <img :src="imgInk3" class="asset-ink pos-ink-3" alt="ink3" />
      <img :src="imgInk4" class="asset-ink pos-ink-4" alt="ink4" />
      <img :src="imgInk5" class="asset-ink pos-ink-5" alt="ink5" />
      <img :src="imgTen1" class="asset-tentacle pos-ten-1" alt="tentacle1" />
      <img :src="imgTen2" class="asset-tentacle pos-ten-2" alt="tentacle2" />
      <img :src="imgTotem" class="asset-totem pos-center-totem" alt="totem" />
    </div>

    <div class="layer-eye-canvas-container">
      <canvas ref="eyeCanvas" class="isolated-eye-engine"></canvas>
    </div>

    <div class="layer-ui-grid">
      
      <aside class="ui-column col-left">
        
        <section class="ui-block block-header">
          <h1 class="main-title">V'ZUOR KHAA-SH'AN</h1>
          <div class="status-line">
            <span class="status-dot"></span> [协议 22408] 观测者系统在线
          </div>
          <div class="timer-box">
            <div class="time-huge">{{ doomTimer }}</div>
            <div class="time-sub">距星辰归位 (2028-12)</div>
          </div>
        </section>

        <section class="ui-block block-metrics">
          <h2 class="block-title">灵魂与肉身刻度</h2>
          <div class="metric-list">
            <div class="m-row">
              <span class="m-label">理智残留 (SAN)</span>
              <span class="m-value txt-blood">{{ sanityIndex.toFixed(4) }}%</span>
            </div>
            <div class="m-row">
              <span class="m-label">业障权重 (KARMA)</span>
              <span class="m-value">{{ karmaWeight.toFixed(2) }}</span>
            </div>
            <div class="m-row">
              <span class="m-label">肉身炼金术</span>
              <span class="m-value txt-bold">3-ON / 2-OFF</span>
            </div>
            <div class="m-row">
              <span class="m-label">神经催化灵药</span>
              <span class="m-value" :class="{'txt-hyper': isHyper}">{{ caffeineLvl }} MG</span>
            </div>
          </div>
        </section>

        <section class="ui-block block-pillars">
          <h2 class="block-title">维度同化进度</h2>
          <div class="pillar-list">
            <div class="p-item">
              <div class="p-head"><span>Ⅰ 逻辑 (C语言 / 数据结构)</span><span>{{ progC }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: progC + '%'}"></div></div>
            </div>
            <div class="p-item">
              <div class="p-head"><span>Ⅱ 根基 (408 计算机基础)</span><span>{{ prog408 }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: prog408 + '%'}"></div></div>
            </div>
            <div class="p-item">
              <div class="p-head"><span>Ⅲ 虚空 (高阶数学)</span><span>{{ progMath }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: progMath + '%'}"></div></div>
            </div>
            <div class="p-item">
              <div class="p-head"><span>Ⅳ 咒语 (考研英语)</span><span>{{ progEng }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: progEng + '%'}"></div></div>
            </div>
          </div>
        </section>

      </aside>

      <main class="ui-column col-center">
        </main>

      <aside class="ui-column col-right">
        
        <section class="ui-block block-logs">
          <h2 class="block-title">阿卡夏记录终端</h2>
          <div class="log-scroll-view" ref="logContainer">
            <div v-for="(log, idx) in logs" :key="idx" class="log-entry" :class="log.type">
              <span class="log-ts">[{{ log.time }}]</span>
              <span class="log-msg">{{ log.msg }}</span>
            </div>
          </div>
        </section>

        <section class="ui-block block-offerings">
          <h2 class="block-title">凡人供奉录</h2>
          <div class="offering-board">
            <div v-for="(msg, i) in messageBoard" :key="i" class="offering-msg">
              <span class="msg-content">"{{ msg.text }}"</span>
            </div>
            <div v-if="messageBoard.length === 0" class="msg-empty">
              ... 等待灵魂的低语 ...
            </div>
          </div>
        </section>

        <section class="ui-block block-cli">
          <div class="cli-input-container">
            <span class="cli-prompt">root@vzuor:~#</span>
            <input 
              v-model="cmdInput" 
              @keyup.enter="executeCommand"
              class="cli-input-element" 
              placeholder="输入 'vocab [数]' 或 'msg [内容]'..."
              spellcheck="false" autocomplete="off"
            />
          </div>
        </section>

      </aside>
    </div>

    <div class="layer-death-shroud" v-show="isDead">
      <div class="death-content">
        <h1 class="death-title">因果重置中</h1>
        <p class="death-subtitle">正在重新刻印世界线...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
/**
 * ============================================================================
 * [内核卷轴 I：本地绝对素材绑定]
 * ============================================================================
 * 强制使用 import 挂载本地路径。绝不允许 URL 解析错误导致白屏。
 */
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

import imgPaper from './assets/paper.jpg'
import imgTotem from './assets/totem.jpg'
import imgInk1 from './assets/ink1.jpg'
import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg'
import imgInk4 from './assets/ink4.jpg'
import imgInk5 from './assets/ink5.jpg'
import imgTen1 from './assets/tentacle1.jpg'
import imgTen2 from './assets/tentacle2.jpg'

/**
 * ============================================================================
 * [内核卷轴 II：企业级数据持久化中间件 (DB Middleware)]
 * ============================================================================
 * 模拟复杂的数据库交互，保证状态绝对稳定。
 * 未来迁移后端时，仅需替换 asyncSync 方法内的逻辑。
 */
class RitualDB {
  static NAMESPACE = 'vzuor_v14_'

  /** @returns {number} */
  static getFloat(key, defaultVal) {
    const val = localStorage.getItem(this.NAMESPACE + key)
    return val !== null ? parseFloat(val) : defaultVal
  }

  /** @param {string} key @param {number|string} val */
  static setVal(key, val) {
    localStorage.setItem(this.NAMESPACE + key, val.toString())
  }

  /** @returns {Array} */
  static fetchMessages() {
    try {
      const raw = localStorage.getItem(this.NAMESPACE + 'messages')
      return raw ? JSON.parse(raw) : []
    } catch (err) {
      console.error('[DB Error] 留言板读取失败', err)
      return []
    }
  }

  /** @param {Object} msgObj */
  static insertMessage(msgObj) {
    const msgs = this.fetchMessages()
    msgs.push(msgObj)
    if (msgs.length > 5) msgs.shift() // 保持版面极度整洁，最多5条
    localStorage.setItem(this.NAMESPACE + 'messages', JSON.stringify(msgs))
    return msgs
  }

  /** 模拟异步后端交互钩子 */
  static async asyncSync(action, payload) {
    return new Promise((resolve) => {
      setTimeout(() => {
        // console.log(`[Network] ${action} 同步成功:`, payload)
        resolve({ status: 'success', timestamp: Date.now() })
      }, 50)
    })
  }
}

/**
 * ============================================================================
 * [内核卷轴 III：响应式状态树]
 * ============================================================================
 */
// 核心状态
const sanityIndex = ref(RitualDB.getFloat('sanity', 99.9998))
const karmaWeight = ref(RitualDB.getFloat('karma', 23.70))
const deathCount = ref(RitualDB.getFloat('deaths', 0))
const caffeineLvl = ref(200)

// 词汇量 (映射到 53%)
const vocabSiphoned = ref(RitualDB.getFloat('vocab', 2150))
const vocabPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

// 柱神进度
const progC = ref(RitualDB.getFloat('prog_c', 45))
const prog408 = ref(RitualDB.getFloat('prog_408', 15))
const progMath = ref(RitualDB.getFloat('prog_math', 20))
const progEng = ref(RitualDB.getFloat('prog_eng', 35))

// UI 态
const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref('')
const isHyper = ref(false)
const isDead = ref(false)

// 日志系统
const logs = reactive([])
const logContainer = ref(null)

// 留言板系统 (剔除倾斜和重叠，保持极简列表)
const messageBoard = reactive(RitualDB.fetchMessages())

/**
 * ============================================================================
 * [内核卷轴 IV：系统调度与指令执行器]
 * ============================================================================
 */
class SystemDispatcher {
  static getTimestamp() {
    const now = new Date()
    const hh = String(now.getHours()).padStart(2, '0')
    const mm = String(now.getMinutes()).padStart(2, '0')
    const ss = String(now.getSeconds()).padStart(2, '0')
    return `${hh}:${mm}:${ss}`
  }

  static printLog(msg, type = 'info') {
    logs.push({ time: this.getTimestamp(), msg, type })
    if (logs.length > 50) logs.shift()
    nextTick(() => {
      if (logContainer.value) {
        logContainer.value.scrollTop = logContainer.value.scrollHeight
      }
    })
  }

  static applySanityDecay() {
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001 - (deathCount.value * 0.00005))
    RitualDB.setVal('sanity', sanityIndex.value)
  }

  static triggerDeath() {
    isDead.value = true
    deathCount.value++
    RitualDB.setVal('deaths', deathCount.value)
    this.printLog(`执行因果重置。进入死亡轮回 [${deathCount.value}]`, 'alert')
    
    setTimeout(() => {
      isDead.value = false
      sanityIndex.value = 100
      karmaWeight.value += 10.0
      RitualDB.setVal('sanity', 100)
      RitualDB.setVal('karma', karmaWeight.value)
      this.printLog('世界线已重新锚定。', 'sys')
    }, 3000)
  }
}

// 指令入口
const executeCommand = async () => {
  const raw = cmdInput.value.trim()
  if (!raw) return
  SystemDispatcher.printLog(`> ${raw}`, 'echo')

  const parts = raw.split(' ')
  const cmd = parts[0].toLowerCase()
  const arg = parts.slice(1).join(' ')

  try {
    switch (cmd) {
      case 'vocab':
        const vAmt = parseInt(arg)
        if (isNaN(vAmt) || vAmt <= 0) throw new Error('无效的灵魂数量')
        
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + vAmt)
        progEng.value = Math.min(100, progEng.value + (vAmt * 0.05))
        karmaWeight.value = Math.max(0, karmaWeight.value - (vAmt * 0.02))
        
        RitualDB.setVal('vocab', vocabSiphoned.value)
        RitualDB.setVal('prog_eng', progEng.value)
        RitualDB.setVal('karma', karmaWeight.value)
        
        await RitualDB.asyncSync('UPDATE_VOCAB', { val: vocabSiphoned.value })
        SystemDispatcher.printLog(`吞噬完成。同化率跃升至 ${vocabPercent.value}%`, 'success')
        break

      case 'msg':
        if (!arg) throw new Error('虚无的低语无法被记录')
        const newMsg = { id: Date.now(), text: arg }
        const updatedList = RitualDB.insertMessage(newMsg)
        messageBoard.splice(0, messageBoard.length, ...updatedList)
        SystemDispatcher.printLog('供奉已刻印于石板。', 'success')
        break

      case 'dose':
        const dose = parseInt(arg)
        if (isNaN(dose) || dose <= 0) throw new Error('无效的灵药剂量')
        caffeineLvl.value = dose
        if (dose >= 400) {
          isHyper.value = true
          if (window.eyeEngine) window.eyeEngine.setHyper(true)
          SystemDispatcher.printLog('警告：灵药浓度超载！', 'alert')
          setTimeout(() => {
            isHyper.value = false
            if (window.eyeEngine) window.eyeEngine.setHyper(false)
            caffeineLvl.value = 200
            SystemDispatcher.printLog('灵药反噬消退，机体恢复正常。', 'sys')
          }, 6000)
        } else {
          SystemDispatcher.printLog(`灵药注入。当前剂量 ${dose} MG。`, 'info')
        }
        break

      case 'die':
        SystemDispatcher.triggerDeath()
        break

      default:
        karmaWeight.value += 0.5
        RitualDB.setVal('karma', karmaWeight.value)
        throw new Error('未知的深渊指令。业障已增加。')
    }
  } catch (err) {
    SystemDispatcher.printLog(err.message, 'error')
  }

  cmdInput.value = ''
}

/**
 * ============================================================================
 * [内核卷轴 V：独立仿生眼球引擎 (Isolated Canvas Engine)]
 * ============================================================================
 * 彻底剥离背景和图片渲染。此 Canvas 仅用于绘制眼球，确保绝对稳定。
 */
const eyeCanvas = ref(null)

class BionicEye {
  constructor(canvasElement) {
    this.canvas = canvasElement
    this.ctx = canvasElement.getContext('2d', { alpha: true }) // 允许透明背景
    this.w = 0; this.h = 0
    this.dpr = window.devicePixelRatio || 1
    
    // 几何属性
    this.centerX = 0; this.centerY = 0
    this.baseR = 80; this.pupilR = 36
    
    // 物理属性
    this.pupilX = 0; this.pupilY = 0
    this.velX = 0; this.velY = 0
    this.targetX = 0; this.targetY = 0
    this.tension = 0.12; this.damping = 0.75 // 阻尼弹簧算法
    
    // 状态
    this.mouseX = 0; this.mouseY = 0
    this.blinkTimer = 150
    this.isHyper = false
    this.animFrame = null
    
    // 绑定上下文
    this.resize = this.resize.bind(this)
    this.onMouseMove = this.onMouseMove.bind(this)
    this.render = this.render.bind(this)
  }

  init() {
    this.resize()
    window.addEventListener('resize', this.resize)
    window.addEventListener('mousemove', this.onMouseMove)
    this.animFrame = requestAnimationFrame(this.render)
  }

  dispose() {
    window.removeEventListener('resize', this.resize)
    window.removeEventListener('mousemove', this.onMouseMove)
    cancelAnimationFrame(this.animFrame)
  }

  setHyper(val) {
    this.isHyper = val
  }

  resize() {
    // 必须获取容器的真实物理尺寸
    const rect = this.canvas.parentElement.getBoundingClientRect()
    this.w = rect.width
    this.h = rect.height
    
    // DPI 适配，防止模糊
    this.canvas.width = this.w * this.dpr
    this.canvas.height = this.h * this.dpr
    this.ctx.scale(this.dpr, this.dpr)
    
    // 锚定在屏幕中央偏下 (匹配 HTML 中耶稣图腾下方的位置)
    this.centerX = this.w / 2
    this.centerY = this.h * 0.75
    this.pupilX = this.centerX
    this.pupilY = this.centerY
  }

  onMouseMove(e) {
    // 将屏幕鼠标坐标映射到 Canvas 内部坐标
    const rect = this.canvas.getBoundingClientRect()
    this.mouseX = e.clientX - rect.left
    this.mouseY = e.clientY - rect.top
  }

  updatePhysics() {
    // 计算边界向量
    const dx = this.mouseX - this.centerX
    const dy = this.mouseY - this.centerY
    const dist = Math.sqrt(dx * dx + dy * dy)
    const maxDistance = this.baseR - this.pupilR - 6
    
    if (dist > 0) {
      this.targetX = this.centerX + (dx / dist) * Math.min(dist * 0.15, maxDistance)
      this.targetY = this.centerY + (dy / dist) * Math.min(dist * 0.15, maxDistance)
    } else {
      this.targetX = this.centerX
      this.targetY = this.centerY
    }

    // 弹性位移计算
    const ax = (this.targetX - this.pupilX) * this.tension
    const ay = (this.targetY - this.pupilY) * this.tension
    this.velX = (this.velX + ax) * this.damping
    this.velY = (this.velY + ay) * this.damping
    this.pupilX += this.velX
    this.pupilY += this.velY

    // 眨眼状态机
    this.blinkTimer--
    if (this.blinkTimer < -5) {
      this.blinkTimer = Math.random() * 200 + 80
    }
  }

  drawBloodVeins() {
    const ctx = this.ctx
    ctx.lineWidth = 1
    for (let i = 0; i < 36; i++) {
      const angle = (Math.PI * 2 / 36) * i + Math.random() * 0.1
      const innerDist = this.pupilR + 5 + Math.random() * 15
      const outerDist = this.baseR - Math.random() * 5
      
      const startX = this.centerX + Math.cos(angle) * innerDist
      const startY = this.centerY + Math.sin(angle) * innerDist
      const endX = this.centerX + Math.cos(angle) * outerDist
      const endY = this.centerY + Math.sin(angle) * outerDist
      
      ctx.beginPath()
      ctx.moveTo(startX, startY)
      // 添加贝塞尔曲线使血丝弯曲
      const cpX = this.centerX + Math.cos(angle + 0.2) * ((innerDist + outerDist) / 2)
      const cpY = this.centerY + Math.sin(angle + 0.2) * ((innerDist + outerDist) / 2)
      ctx.quadraticCurveTo(cpX, cpY, endX, endY)
      
      ctx.strokeStyle = `rgba(139, 0, 0, ${Math.random() * 0.4 + 0.1})`
      ctx.stroke()
    }
  }

  render() {
    this.ctx.clearRect(0, 0, this.w, this.h)
    this.updatePhysics()

    const ctx = this.ctx

    // 1. 眼白 (Sclera)
    ctx.beginPath()
    ctx.arc(this.centerX, this.centerY, this.baseR, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(235, 232, 222, 0.95)' // 浑浊纸张色
    ctx.fill()
    ctx.lineWidth = 4
    ctx.strokeStyle = '#050505'
    ctx.stroke()

    // 2. 血丝
    this.drawBloodVeins()

    // 3. 瞳孔与数值
    if (this.blinkTimer > 0) {
      // 瞳孔
      ctx.beginPath()
      ctx.arc(this.pupilX, this.pupilY, this.pupilR, 0, Math.PI * 2)
      ctx.fillStyle = this.isHyper ? '#600' : '#0a0a0a'
      ctx.fill()
      
      // 进度雕刻
      ctx.fillStyle = '#E2DED0'
      ctx.font = "bold 32px 'Cinzel', serif"
      ctx.textAlign = "center"
      ctx.textBaseline = "middle"
      ctx.fillText(`${vocabPercent.value}%`, this.pupilX, this.pupilY + 2)
      
      // 活体高光点
      ctx.beginPath()
      ctx.arc(this.pupilX - 12, this.pupilY - 12, 4, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(255, 255, 255, 0.6)'
      ctx.fill()
    } else {
      // 闭眼形态 (一条缝合线)
      ctx.beginPath()
      ctx.moveTo(this.centerX - this.baseR, this.centerY)
      ctx.quadraticCurveTo(this.centerX, this.centerY + 25, this.centerX + this.baseR, this.centerY)
      ctx.lineWidth = 5
      ctx.strokeStyle = '#050505'
      ctx.stroke()
    }

    this.animFrame = requestAnimationFrame(this.render)
  }
}

/**
 * ============================================================================
 * [内核卷轴 VI：生命周期调度]
 * ============================================================================
 */
let chronosTimer
let engineInstance

onMounted(() => {
  SystemDispatcher.printLog('系统初始化...', 'sys')
  
  // 启动仿生眼球引擎
  nextTick(() => {
    if (eyeCanvas.value) {
      engineInstance = new BionicEye(eyeCanvas.value)
      engineInstance.init()
      window.eyeEngine = engineInstance // 暴露给指令系统
      SystemDispatcher.printLog('光学眼球矩阵已连接。', 'success')
    }
  })

  // 宿命时钟循环
  chronosTimer = setInterval(() => {
    const deadline = new Date('2028-12-23T08:30:00')
    const diff = deadline - Date.now()
    if (diff <= 0) {
      doomTimer.value = "DESTINY ARRIVED"
      clearInterval(chronosTimer)
      return
    }
    const d = Math.floor(diff / 86400000).toString().padStart(3, '0')
    const h = Math.floor((diff / 3600000) % 24).toString().padStart(2, '0')
    const m = Math.floor((diff / 60000) % 60).toString().padStart(2, '0')
    const s = Math.floor((diff / 1000) % 60).toString().padStart(2, '0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    SystemDispatcher.applySanityDecay()
  }, 1000)
})

onBeforeUnmount(() => {
  clearInterval(chronosTimer)
  if (engineInstance) engineInstance.dispose()
})

</script>

<style scoped>
/**
 * ============================================================================
 * [终极绝界织物：CSS 架构]
 * ============================================================================
 * 准则 1：强制铺满 100vw/100vh，无视一切滚动条和缩放问题。
 * 准则 2：UI 采用现代 Flex/Grid 布局，绝对禁止元素重叠。
 * 准则 3：使用 mix-blend-mode: multiply 物理消灭白底，文字必须贴合羊皮纸。
 */

@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Zhi+Mang+Xing&display=swap');

:root {
  --ink-primary: #121412;
  --ink-secondary: #333333;
  --blood-red: #8B0000;
  --bg-fallback: #E2DED0;
  
  --font-title: 'Cinzel', serif;
  --font-mono: 'Space Mono', 'Microsoft YaHei', monospace;
  --font-script: 'Zhi Mang Xing', cursive;
}

/* 视口锁死 */
* { box-sizing: border-box; }
body, html {
  margin: 0; padding: 0;
  width: 100vw; height: 100vh;
  overflow: hidden; /* 核心：禁止滚动，锁死维度 */
  background-color: var(--bg-fallback);
}

.hub-container {
  position: relative;
  width: 100vw; height: 100vh;
  font-family: var(--font-mono);
  color: var(--ink-primary);
}

/* -----------------------------------------------------
   [光学第一层：物理背景]
------------------------------------------------------ */
.layer-base-parchment {
  position: absolute; inset: 0;
  width: 100vw; height: 100vh;
  object-fit: cover; /* 保证图片不变形且撑满屏幕 */
  z-index: 0;
}
.layer-global-vignette {
  position: absolute; inset: 0;
  background: radial-gradient(circle at center, transparent 40%, rgba(15, 10, 5, 0.45) 100%);
  z-index: 1; pointer-events: none;
}

/* -----------------------------------------------------
   [光学第二层：墨迹与图腾]
   核心：原生 img 标签 + multiply = 永不崩溃的完美去底
------------------------------------------------------ */
.layer-art-assets {
  position: absolute; inset: 0;
  z-index: 5; pointer-events: none;
}
.layer-art-assets img {
  position: absolute;
  mix-blend-mode: multiply; /* 光学消灭白底魔法 */
}
/* 通过百分比进行模糊定位，不影响中心 */
.pos-ink-1 { top: -5%; left: -5%; width: 35vw; opacity: 0.8; }
.pos-ink-2 { bottom: -10%; right: -5%; width: 45vw; opacity: 0.85; }
.pos-ink-3 { top: 10%; right: 5%; width: 25vw; opacity: 0.6; }
.pos-ink-4 { bottom: 5%; left: 10%; width: 30vw; opacity: 0.7; }
.pos-ink-5 { top: 40%; left: 40%; width: 20vw; opacity: 0.3; }
.pos-ten-1 { bottom: 15%; right: 15%; width: 20vw; opacity: 0.2; }
.pos-ten-2 { top: 20%; left: 10%; width: 18vw; opacity: 0.3; }

/* 耶稣中央祭坛，使用 Flex 居中 */
.pos-center-totem {
  top: 5%; left: 50%;
  transform: translateX(-50%);
  height: 85vh; /* 高度为主，宽度自适应 */
  width: auto;
  opacity: 0.95;
}

/* -----------------------------------------------------
   [光学第三层：独立眼球画布]
------------------------------------------------------ */
.layer-eye-canvas-container {
  position: absolute; inset: 0;
  z-index: 10;
  pointer-events: none; /* 让鼠标事件穿透给底层 window 监听 */
}
.isolated-eye-engine {
  width: 100vw; height: 100vh;
  display: block;
}

/* -----------------------------------------------------
   [光学第四层：稳定数据 UI 矩阵]
   摒弃 absolute 乱飞，使用极其稳定的 Flex 布局
------------------------------------------------------ */
.layer-ui-grid {
  position: absolute; inset: 0;
  z-index: 20;
  display: flex;
  justify-content: space-between; /* 左右分列 */
  padding: 2.5rem 3.5rem;
  pointer-events: none; /* 允许点击穿透空白区域 */
}

/* UI 基础列：约束宽度，内部元素上下排列 */
.ui-column {
  width: 380px;
  display: flex;
  flex-direction: column;
  justify-content: space-between; /* 上下对齐 */
  height: 100%;
}
/* 中央预留空位 */
.col-center { flex: 1; }

/* UI 基础块：禁忌所有的 background，必须透明 */
.ui-block {
  pointer-events: auto;
  background: transparent !important;
  border: none !important;
  box-shadow: none !important;
  backdrop-filter: none !important;
  mix-blend-mode: multiply; /* 文字烧录感 */
}
.block-title {
  font-size: 1.15rem;
  font-weight: bold;
  letter-spacing: 2px;
  border-bottom: 2px solid var(--ink-primary);
  padding-bottom: 6px;
  margin: 0 0 1.2rem 0;
  text-transform: uppercase;
}

/* -- 模块细节 -- */

/* 1. 头部标题 */
.main-title {
  font-family: var(--font-title);
  font-size: 3.5rem; font-weight: 900;
  margin: 0; line-height: 1.1;
  letter-spacing: 1px;
}
.status-line {
  display: flex; align-items: center; gap: 8px;
  font-size: 0.85rem; font-weight: bold; color: var(--ink-secondary);
  margin-top: 8px;
}
.status-dot { width: 8px; height: 8px; background: var(--blood-red); border-radius: 50%; animation: pulse-dot 1.5s infinite; }
.timer-box { margin-top: 1.5rem; }
.time-huge { font-size: 2.8rem; font-weight: bold; letter-spacing: -1px; }
.time-sub { font-size: 0.8rem; font-weight: bold; color: var(--ink-secondary); }

/* 2. 肉身监控 */
.metric-list { display: flex; flex-direction: column; gap: 10px; font-size: 0.95rem; font-weight: bold; }
.m-row { display: flex; justify-content: space-between; border-bottom: 1px dashed rgba(0,0,0,0.2); padding-bottom: 4px; }
.txt-blood { color: var(--blood-red); }
.txt-hyper { color: #d00; text-shadow: 0 0 8px rgba(200,0,0,0.5); font-size: 1.1rem; }

/* 3. 四大柱神 */
.pillar-list { display: flex; flex-direction: column; gap: 14px; font-size: 0.9rem; font-weight: bold; }
.p-item { display: flex; flex-direction: column; gap: 6px; }
.p-head { display: flex; justify-content: space-between; }
.p-bar-bg { width: 100%; height: 6px; border: 1px solid var(--ink-primary); }
.p-bar-fill { height: 100%; background: var(--ink-primary); transition: width 0.5s ease-out; }

/* 4. 日志终端 */
.log-scroll-view {
  height: 200px; overflow-y: auto;
  font-size: 0.75rem;
  display: flex; flex-direction: column; gap: 6px;
  padding-left: 10px; border-left: 2px solid var(--ink-primary);
}
.log-scroll-view::-webkit-scrollbar { width: 3px; }
.log-scroll-view::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.4); }
.log-entry { line-height: 1.4; word-break: break-all; }
.log-ts { color: var(--ink-secondary); margin-right: 6px; }
.info { color: var(--ink-primary); }
.sys { color: var(--ink-secondary); font-style: italic; }
.echo { color: #444; font-style: italic; }
.success { color: #005500; font-weight: bold; }
.error, .alert { color: var(--blood-red); font-weight: bold; }

/* 5. 留言板 (稳定列表，摒弃乱飞) */
.block-offerings { margin-bottom: 1rem; }
.offering-board {
  display: flex; flex-direction: column; gap: 12px;
}
.offering-msg {
  font-family: var(--font-script);
  font-size: 1.8rem;
  color: #050505;
  line-height: 1.2;
}
.msg-empty { font-size: 0.85rem; color: var(--ink-secondary); font-style: italic; }

/* 6. 指令输入 (唯一需要轻微底色的地方，保证输入可见，但不廉价) */
.block-cli {
  mix-blend-mode: normal !important; /* 取消叠底，防看不清 */
}
.cli-input-container {
  display: flex; align-items: center; gap: 10px;
  background: rgba(226, 222, 208, 0.7); /* 羊皮纸色微透明 */
  padding: 12px 15px;
  border: 2px solid var(--ink-primary);
}
.cli-prompt { font-weight: 900; font-size: 1.1rem; color: #004400; }
.cli-input-element {
  flex: 1; border: none; background: transparent; outline: none;
  font-size: 1.2rem; font-family: inherit; font-style: italic;
  color: var(--ink-primary);
  border-bottom: 1px dashed rgba(0,0,0,0.3); transition: 0.2s;
}
.cli-input-element:focus { border-bottom: 1px solid var(--ink-primary); }
.cli-input-element::placeholder { color: #666; font-size: 0.9rem; font-style: normal; }
.cli-input-element:disabled { opacity: 0.4; }

/* -----------------------------------------------------
   [死亡遮罩]
------------------------------------------------------ */
.layer-death-shroud {
  position: fixed; inset: 0; z-index: 9999;
  background: #020202; display: flex; justify-content: center; align-items: center;
}
.death-content { text-align: center; }
.death-title { color: #eee; font-family: var(--font-title); font-size: 4rem; letter-spacing: 10px; margin: 0; text-shadow: 0 0 15px #a00; }
.death-subtitle { color: #666; font-size: 1.2rem; letter-spacing: 5px; margin-top: 20px; animation: pulse-opacity 1s infinite; }

/* 动画库 */
@keyframes pulse-dot { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
@keyframes pulse-opacity { 0%, 100% { opacity: 0.2; } 50% { opacity: 1; } }
</style>