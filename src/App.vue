/* [阿卡姆飞升魔典 V9.0 // 终极降临与绝对自适应版]
   [核心法则：1200行逻辑深度 | 纯Canvas像素融合 | 极致流体响应式排版]
   [契约者：武少康 // 目标：22408 飞升]
*/
<template>
  <div id="vzuor-nexus" class="nexus-root" :class="[realmState, sanityStageClass]">
    
    <svg width="0" height="0" class="occult-svg-defs">
      <defs>
        <filter id="void-glitch">
          <feTurbulence type="fractalNoise" baseFrequency="0.05 0.95" numOctaves="3" result="noise" />
          <feColorMatrix type="matrix" values="1 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 1 0" in="noise" result="coloredNoise" />
          <feComposite operator="in" in="coloredNoise" in2="SourceGraphic" result="composite" />
          <feBlend mode="multiply" in="composite" in2="SourceGraphic" />
        </filter>
        <filter id="flesh-melt">
          <feGaussianBlur in="SourceGraphic" stdDeviation="1.5" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 15 -6" result="melt" />
          <feBlend mode="multiply" in="SourceGraphic" in2="melt" />
        </filter>
      </defs>
    </svg>

    <div class="static-noise"></div>
    <div class="void-scanlines"></div>

    <canvas ref="mainCanvas" class="abyssal-canvas"></canvas>

    <div class="interface-grid">
      
      <header class="grid-header">
        <div class="title-construct">
          <h1 class="eldritch-title" :data-text="pageTitle">{{ pageTitle }}</h1>
          <div class="status-monitor">
            <span class="pulse-sig"></span>
            <span class="mono-label">KERN_V9.0 // ABYSSAL_ALTAR // [ {{ systemStatus }} ]</span>
          </div>
        </div>
        <div class="chronos-construct">
          <div class="doom-timer" :class="{'shake-timer': karmaLoad > 80}">{{ doomTimer }}</div>
          <div class="timer-label">UNTIL_THE_STARS_ARE_RIGHT (2028-12)</div>
        </div>
      </header>

      <aside class="grid-left panel-glass">
        <h2 class="zone-title">THE_PILLARS</h2>
        <div class="pillar-matrix">
          <div class="pillar-node" :class="{'active': activePillar === 'C_LANG'}" @click="focusPillar('C_LANG')">
            <span class="node-icon">Ⅰ</span>
            <div class="node-data">
              <div class="node-name">FLESH (C_LANGUAGE)</div>
              <div class="node-status">{{ getPillarStatus('C_LANG') }}</div>
            </div>
            <div class="node-corruption-bar"><div class="bar-fill" :style="{width: pillarProgress.C_LANG + '%'}"></div></div>
          </div>
          
          <div class="pillar-node" :class="{'active': activePillar === 'DATA_STRUCT'}" @click="focusPillar('DATA_STRUCT')">
            <span class="node-icon">Ⅱ</span>
            <div class="node-data">
              <div class="node-name">BONE (DATA_STRUCTURES)</div>
              <div class="node-status">{{ getPillarStatus('DATA_STRUCT') }}</div>
            </div>
            <div class="node-corruption-bar"><div class="bar-fill" :style="{width: pillarProgress.DATA_STRUCT + '%'}"></div></div>
          </div>
          
          <div class="pillar-node" :class="{'active': activePillar === 'MATH'}" @click="focusPillar('MATH')">
            <span class="node-icon">Ⅲ</span>
            <div class="node-data">
              <div class="node-name">VOID (MATH)</div>
              <div class="node-status">{{ getPillarStatus('MATH') }}</div>
            </div>
            <div class="node-corruption-bar"><div class="bar-fill" :style="{width: pillarProgress.MATH + '%'}"></div></div>
          </div>
        </div>
        
        <div class="forge-stats mt-auto">
          <h2 class="zone-title">FLESH_ALCHEMY</h2>
          <div class="forge-data">
            <div class="forge-label">CYCLE: 3-ON / 2-OFF</div>
            <div class="forge-bar-wrap">
              <div class="forge-bar"><div class="forge-progress" style="width: 60%"></div></div>
            </div>
            <div class="forge-dose-row">
              <div class="label-muted">ELIXIR_DOSE:</div>
              <div class="dose-val" :class="{'high-dose text-hyper': isHyper}">{{ caffeineLvl }} MG</div>
            </div>
          </div>
        </div>
      </aside>

      <main class="grid-center">
        <div class="altar-focus-zone">
          <div class="abyssal-water-pool">
            <div class="water-wave" :style="{ height: vocabProgressPercentage + '%' }"></div>
            <div class="pool-glass-overlay"></div>
          </div>
          <div class="siphon-readout">
            <div class="massive-percent">
              <span class="num">{{ vocabProgressPercentage }}</span><span class="unit">%</span>
            </div>
            <div class="vocab-total">{{ vocabSiphoned }} / 4000 SOULS</div>
            <div class="daily-sacrifice" v-if="dailyVocab > 0">TODAY_SACRIFICE: +{{ dailyVocab }}</div>
          </div>
        </div>
      </main>

      <aside class="grid-right panel-glass">
        <h2 class="zone-title">SOUL_METRICS</h2>
        <div class="metrics-matrix">
          <div class="metric-row">
            <span class="label">SANITY_REMNANT:</span>
            <span class="value text-alert">{{ sanityIndex.toFixed(4) }}%</span>
          </div>
          <div class="metric-row">
            <span class="label">KARMA_WEIGHT:</span>
            <span class="value" :class="{'text-hyper': karmaLoad > 50}">{{ karmaLoad.toFixed(2) }}</span>
          </div>
          <div class="metric-row">
            <span class="label">CYCLE_OF_DEATH:</span>
            <span class="value text-crimson">[{{ deathReturns }}]</span>
          </div>
        </div>

        <h2 class="zone-title mt-4">AKASHIC_RECORDS</h2>
        <div class="terminal-logs" ref="logBox">
          <div v-for="(log, idx) in cmdHistory" :key="idx" class="log-entry" :class="log.type">
            <span class="log-time">[{{ log.time }}]</span>
            <span class="log-msg" v-html="log.msg"></span>
          </div>
        </div>
      </aside>

      <footer class="grid-footer panel-glass">
        <div class="input-cli">
          <span class="cli-prefix">root@vzuor:~#</span>
          <input 
            v-model="currentCmd"
            @keyup.enter="handleCommand"
            @keyup.up="recallCommand(-1)"
            @keyup.down="recallCommand(1)"
            class="cli-input"
            placeholder="AWAITING SACRIFICE... (type 'chant' for spells)"
            spellcheck="false"
            autocomplete="off"
            :disabled="isDeathReturning"
          />
        </div>
      </footer>
    </div>

    <div class="death-overlay" v-if="isDeathReturning">
      <div class="death-text-container">
        <div class="glitch-word">KARMA</div>
        <div class="glitch-word text-crimson">OVERFLOW</div>
        <div class="sub-death">REWEAVING_THE_THREADS_OF_FATE...</div>
      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ============================================================================
// [卷一：资源动态加载与真名解析 (Vite Asset Loading)]
// ============================================================================
const getAssetUrl = (name) => new URL(`./assets/${name}`, import.meta.url).href

const loadTexture = (src) => {
  return new Promise((resolve) => {
    const img = new Image()
    img.src = src
    img.onload = () => resolve(img)
    img.onerror = () => { console.error(`Failed to load: ${src}`); resolve(null) }
  })
}

// ============================================================================
// [卷二：状态机与因果持久化 (State Engine)]
// ============================================================================
const loadEngram = (k, def) => Number(localStorage.getItem(`vzuor_v9_${k}`)) || def
const saveEngram = (k, val) => localStorage.setItem(`vzuor_v9_${k}`, val)

const doomTimer = ref('000D | 00:00:00')
const vocabSiphoned = ref(loadEngram('vocab', 2150))
const dailyVocab = ref(loadEngram('daily_vocab', 0))
const karmaLoad = ref(loadEngram('karma', 23.7))
const caffeineLvl = ref(200)
const sanityIndex = ref(loadEngram('sanity', 99.9999))
const deathReturns = ref(loadEngram('deaths', 0))

const activePillar = ref('C_LANG')
const isGazed = ref(false)

const pillarProgress = reactive({
  C_LANG: loadEngram('prog_c', 35),
  DATA_STRUCT: loadEngram('prog_ds', 12),
  MATH: loadEngram('prog_math', 5)
})

const vocabProgressPercentage = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

const sanityStageClass = computed(() => {
  if (sanityIndex.value > 80) return 'sanity-high'
  if (sanityIndex.value > 40) return 'sanity-med'
  return 'sanity-low' // 触发 flesh-melt 滤镜
})

const pageTitle = ref("V'ZUOR KHAA-SH'AN")
const systemStatus = ref('SOUL_BINDING_ACTIVE')
const realmState = ref('realm-stable')
const isDeathReturning = ref(false)
const isSanityBroken = ref(false)

// ============================================================================
// [卷三：终端解析器 (The Grimoire CLI)]
// ============================================================================
const currentCmd = ref('')
const commandHistoryRing = []
let historyIndex = -1

const cmdHistory = reactive([
  { time: getSysTime(), type: 'sys', msg: 'V9.0 终极协议唤醒。像素引擎已接管，布局锁定。' },
  { time: getSysTime(), type: 'sys', msg: '因果律防线部署。所有图像白底已强行抹杀。' }
])
const logBox = ref(null)

function getSysTime() {
  const d = new Date()
  return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}`
}

const getPillarStatus = (pillar) => {
  const p = pillarProgress[pillar]
  if (p < 20) return "INITIAL_CONTACT"
  if (p < 60) return "DEEP_CORRUPTION"
  if (p < 100) return "RECONSTRUCTING"
  return "ASCENDED"
}

const focusPillar = (pillar) => {
  activePillar.value = pillar
  pushLog(`神识已聚焦于柱神：[${pillar}]。`, 'sys')
}

const pushLog = (msg, type = 'info') => {
  cmdHistory.push({ time: getSysTime(), type, msg })
  if (cmdHistory.length > 80) cmdHistory.shift()
  nextTick(() => { if (logBox.value) logBox.value.scrollTop = logBox.value.scrollHeight })
}

const recallCommand = (dir) => {
  if (commandHistoryRing.length === 0) return
  historyIndex += dir
  if (historyIndex < 0) historyIndex = 0
  if (historyIndex >= commandHistoryRing.length) historyIndex = commandHistoryRing.length - 1
  currentCmd.value = commandHistoryRing[commandHistoryRing.length - 1 - historyIndex]
}

const handleCommand = () => {
  const rawCmd = currentCmd.value.trim()
  if (!rawCmd) return
  
  commandHistoryRing.push(rawCmd)
  historyIndex = -1
  pushLog(`> ${rawCmd}`, 'echo')
  
  const args = rawCmd.toLowerCase().split(' ')
  const rootCmd = args[0]

  switch (rootCmd) {
    case 'chant':
    case 'help':
      pushLog('可诵读之咒 (Chants):', 'sys')
      pushLog(' - vocab [num]   - 吞噬考研词汇灵魂 (例: vocab 50)', 'sys')
      pushLog(' - dose [num]    - 饮下灵药催化肉身 (例: dose 400)', 'sys')
      pushLog(' - focus [name]  - 转移献祭焦点 (c/data/math)', 'sys')
      pushLog(' - study [num]   - 献祭理智提升当前柱神进度', 'sys')
      pushLog(' - cleanse       - 消耗词汇量洗刷业障', 'sys')
      pushLog(' - vzuor         - [禁忌] 呼唤真名', 'alert')
      pushLog(' - die           - 割裂喉咙，执行死亡回归', 'alert')
      break

    case 'vocab':
      const amt = parseInt(args[1])
      if (isNaN(amt) || amt <= 0) {
        pushLog('献祭失败：虚无的数量。', 'error')
      } else {
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
        dailyVocab.value += amt
        saveEngram('vocab', vocabSiphoned.value)
        saveEngram('daily_vocab', dailyVocab.value)
        karmaLoad.value = Math.max(0, karmaLoad.value - (amt * 0.02))
        saveEngram('karma', karmaLoad.value)
        sanityIndex.value = Math.min(100, sanityIndex.value + (amt * 0.005))
        saveEngram('sanity', sanityIndex.value)
        pushLog(`灵魂已剥夺。水位上升。进度: <span class="text-bone">${vocabSiphoned.value}/4000</span>`, 'success')
      }
      break

    case 'dose':
      const dose = parseInt(args[1])
      if (!isNaN(dose)) {
        caffeineLvl.value = dose
        if (dose >= 400) triggerHyperDrive()
        else pushLog(`肉身机能锁定在 ${dose}mg 阈值。`, 'info')
      }
      break

    case 'study':
      const stAmt = parseInt(args[1])
      if (!isNaN(stAmt) && stAmt > 0) {
        if (sanityIndex.value < stAmt * 0.5) {
          pushLog('理智不足，强行研究将导致灵魂撕裂。', 'alert')
        } else {
          sanityIndex.value -= stAmt * 0.5
          pillarProgress[activePillar.value] = Math.min(100, pillarProgress[activePillar.value] + stAmt)
          saveEngram(`prog_${activePillar.value.toLowerCase().split('_')[0]}`, pillarProgress[activePillar.value])
          pushLog(`消耗理智。对 [${activePillar.value}] 理解加深 ${stAmt}%。`, 'success')
        }
      }
      break

    case 'cleanse':
      if (vocabSiphoned.value >= 100) {
        vocabSiphoned.value -= 100
        karmaLoad.value = Math.max(0, karmaLoad.value - 15)
        pushLog('献祭 100 个灵魂碎片，洗刷业障。', 'success')
      } else pushLog('灵魂不足以取悦深渊。', 'error')
      break

    case 'vzuor': triggerSanityBreak(); break
    case 'die': triggerDeathReturn(); break
    default:
      karmaLoad.value += 1.5; saveEngram('karma', karmaLoad.value)
      pushLog(`呢喃无意义的音节。业障增加。`, 'error')
  }
  currentCmd.value = ''
}

const triggerHyperDrive = () => {
  realmState.value = 'realm-hyper'
  pushLog('警告：灵药过载！血肉正在溶解...', 'error')
  if(window.vzuorEngine) window.vzuorEngine.setHyper(true)
  setTimeout(() => {
    realmState.value = 'realm-stable'
    caffeineLvl.value = 200
    if(window.vzuorEngine) window.vzuorEngine.setHyper(false)
    pushLog('灵药反噬结束。', 'sys')
    sanityIndex.value -= 2.0
  }, 8000)
}

const triggerSanityBreak = () => {
  isSanityBroken.value = true
  pushLog('<span class="text-crimson">真名回响。不可名状之物撕裂现实。</span>', 'alert')
  sanityIndex.value -= 30.0
  setTimeout(() => isSanityBroken.value = false, 5000)
}

const triggerDeathReturn = () => {
  isDeathReturning.value = true
  deathReturns.value += 1
  saveEngram('deaths', deathReturns.value)
  karmaLoad.value += 20.0
  sanityIndex.value = 100.0
  pushLog(`启动第 ${deathReturns.value} 次因果重置。`, 'error')
  setTimeout(() => isDeathReturning.value = false, 4000)
}

// ============================================================================
// [卷四：高维物理引擎 (The Ultimate Canvas Engine)]
// 响应式核心：所有坐标和尺寸按屏幕比例实时计算
// ============================================================================
const mainCanvas = ref(null)
let ctx, w, h, time = 0, animationId
let isEngineHyper = false

// 预加载库
const Assets = {}

// 1. IK 反向运动学触手 (The Living Limbs)
class ElderLimb {
  constructor(segments, baseLength, color) {
    this.segments = segments; this.baseLength = baseLength
    this.joints = []
    for(let i=0; i<segments; i++) this.joints.push({x: 0, y: 0})
    this.color = color
    this.noiseOffset = Math.random() * 1000
  }
  manifest(baseX, baseY, targetX, targetY, scale) {
    const segLength = this.baseLength * scale
    this.joints[this.segments-1] = { x: targetX, y: targetY }
    for(let i = this.segments - 2; i >= 0; i--) {
      const dx = this.joints[i].x - this.joints[i+1].x
      const dy = this.joints[i].y - this.joints[i+1].y
      const dist = Math.sqrt(dx*dx + dy*dy)
      const ratio = segLength / (dist || 1)
      this.joints[i].x = this.joints[i+1].x + dx * ratio
      this.joints[i].y = this.joints[i+1].y + dy * ratio
    }
    this.joints[0] = { x: baseX, y: baseY }
    for(let i = 1; i < this.segments; i++) {
      const dx = this.joints[i].x - this.joints[i-1].x
      const dy = this.joints[i].y - this.joints[i-1].y
      const dist = Math.sqrt(dx*dx + dy*dy)
      const ratio = segLength / (dist || 1)
      this.joints[i].x = this.joints[i-1].x + dx * ratio
      this.joints[i].y = this.joints[i-1].y + dy * ratio
    }
  }
  draw(ctx, time, isHyper, scale) {
    ctx.beginPath()
    ctx.moveTo(this.joints[0].x, this.joints[0].y)
    for (let i = 1; i < this.segments; i++) {
      const wob = Math.sin(time * (isHyper?4:1) + i * 0.4 + this.noiseOffset) * (isHyper?4:1.5) * scale
      const dx = this.joints[i].x - this.joints[i-1].x
      const dy = this.joints[i].y - this.joints[i-1].y
      const angle = Math.atan2(dy, dx)
      const px = this.joints[i].x + Math.sin(angle) * wob
      const py = this.joints[i].y - Math.cos(angle) * wob
      ctx.lineTo(px, py)
    }
    ctx.strokeStyle = this.color
    ctx.lineWidth = (isHyper ? 5 : 2.5) * scale
    ctx.lineCap = 'round'
    ctx.stroke()
  }
}

// 2. 渊眚凝视 (Void Eyes)
class VoidEye {
  constructor(xRatio, yRatio, baseRadius) {
    this.xRatio = xRatio; this.yRatio = yRatio; this.baseR = baseRadius
    this.blinkTimer = Math.random() * 100
  }
  draw(ctx, w, h, mx, my, scale) {
    const x = w * this.xRatio; const y = h * this.yRatio
    const r = this.baseR * scale
    const dx = mx - x; const dy = my - y
    const dist = Math.sqrt(dx*dx + dy*dy)
    const px = x + (dx / (dist||1)) * Math.min(dist * 0.1, r * 0.5)
    const py = y + (dy / (dist||1)) * Math.min(dist * 0.1, r * 0.5)
    
    this.blinkTimer--
    if (this.blinkTimer < -5) this.blinkTimer = Math.random() * 200 + 50

    ctx.save()
    ctx.beginPath(); ctx.arc(x, y, r, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(230, 220, 210, 0.8)'; ctx.fill()
    ctx.lineWidth = 2 * scale; ctx.strokeStyle = 'rgba(20, 20, 20, 0.9)'; ctx.stroke()
    
    if (this.blinkTimer > 0) {
      ctx.beginPath(); ctx.arc(px, py, r * 0.4, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(10, 5, 5, 0.95)'; ctx.fill()
      ctx.beginPath(); ctx.arc(px, py, r * 0.15, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(180, 20, 20, 0.8)'; ctx.fill()
    } else {
      ctx.beginPath(); ctx.moveTo(x - r, y); ctx.lineTo(x + r, y); ctx.stroke()
    }
    ctx.restore()
  }
}

let elderLimbs = [], voidEyes = []
let mx = 0, my = 0

const bootVoidEngine = async () => {
  if (!mainCanvas.value) return
  ctx = mainCanvas.value.getContext('2d')
  
  const resize = () => {
    w = window.innerWidth; h = window.innerHeight
    mainCanvas.value.width = w; mainCanvas.value.height = h
  }
  resize(); window.addEventListener('resize', resize)

  window.addEventListener('mousemove', (e) => { mx = e.clientX; my = e.clientY })

  pushLog('连接阿卡夏资源库...', 'sys')
  // 异步加载所有神圣素材
  const imgNames = ['paper.jpg', 'totem.jpg', 'ink1.jpg', 'ink2.jpg', 'ink3.jpg', 'ink4.jpg', 'ink5.jpg', 'tentacle1.jpg', 'tentacle2.jpg']
  for (let name of imgNames) {
    Assets[name] = await loadTexture(getAssetUrl(name))
  }
  pushLog('素材解析完成。像素融合矩阵上线。', 'success')

  // 初始化生物
  for (let i = 0; i < 8; i++) elderLimbs.push(new ElderLimb(12, 25, 'rgba(15, 18, 15, 0.6)'))
  voidEyes.push(new VoidEye(0.15, 0.25, 20)); voidEyes.push(new VoidEye(0.85, 0.75, 25)); voidEyes.push(new VoidEye(0.8, 0.2, 15))

  window.vzuorEngine = { setHyper: (val) => isEngineHyper = val }

  const renderLoop = () => {
    ctx.clearRect(0, 0, w, h)
    const scale = Math.min(w, h) / 1080 // 全局缩放因子

    // 1. 绘制底层世界 (羊皮纸)
    if (Assets['paper.jpg']) ctx.drawImage(Assets['paper.jpg'], 0, 0, w, h)

    // 开启绝对混合模式：正片叠底。这行代码将彻底消灭接下来所有图片的白底！
    ctx.globalCompositeOperation = 'multiply'

    // 2. 绘制散落的墨迹与触手 (自适应边缘布局)
    const drawAsset = (img, x, y, dw, dh, alpha=1) => {
      if(!img) return; ctx.globalAlpha = alpha; ctx.drawImage(img, x, y, dw, dh); ctx.globalAlpha = 1
    }
    
    // 将图片布局在屏幕四周，尺寸随屏幕比例缩放
    drawAsset(Assets['ink1.jpg'], -w*0.05, -h*0.05, w*0.3, h*0.4, 0.85)
    drawAsset(Assets['ink2.jpg'], w*0.7, h*0.6, w*0.35, h*0.45, 0.9)
    drawAsset(Assets['ink3.jpg'], -w*0.02, h*0.6, w*0.25, h*0.35, 0.7)
    drawAsset(Assets['ink5.jpg'], w*0.75, -h*0.05, w*0.25, h*0.3, 0.6)
    
    // 潜伏的触手素描
    drawAsset(Assets['tentacle1.jpg'], w*0.1, h*0.3, w*0.2, h*0.3, 0.2)
    drawAsset(Assets['tentacle2.jpg'], w*0.7, h*0.2, w*0.25, h*0.35, 0.3)

    // 3. 绘制核心祭坛 (耶稣受难图)
    // 逻辑：强制居中，大小取屏幕宽度的 35% 或 500px 的极小值，高度按原图比例自适应。
    if (Assets['totem.jpg']) {
      const tImg = Assets['totem.jpg']
      const tWidth = Math.min(w * 0.35, 500)
      const tHeight = tWidth * (tImg.height / tImg.width)
      const tX = w/2 - tWidth/2 + (isEngineHyper ? (Math.random()-0.5)*10 : 0)
      const tY = h/2 - tHeight/2 + (isEngineHyper ? (Math.random()-0.5)*10 : 0)
      drawAsset(tImg, tX, tY, tWidth, tHeight, 1.0)
    }

    // 狂化时的额外血迹喷溅
    if (isEngineHyper) drawAsset(Assets['ink4.jpg'], w*0.2, h*0.2, w*0.6, h*0.6, Math.abs(Math.sin(time*2)))

    // 恢复正常混合模式，用于绘制触手和眼睛
    ctx.globalCompositeOperation = 'source-over'

    // 4. 绘制 IK 触手 (以屏幕中心为锚点)
    elderLimbs.forEach((limb, idx) => {
      let tx, ty
      if (isEngineHyper) {
        tx = w/2 + Math.cos(time*3 + idx) * 400 * scale
        ty = h/2 + Math.sin(time*4 + idx) * 400 * scale
      } else {
        // 追踪鼠标，带一点延迟阻尼
        tx = mx + Math.cos(time*0.5 + idx) * 150 * scale
        ty = my + Math.sin(time*0.6 + idx) * 150 * scale
      }
      limb.manifest(w/2, h/2, tx, ty, scale)
      limb.color = isEngineHyper ? 'rgba(139, 0, 0, 0.8)' : 'rgba(15, 18, 15, 0.6)'
      limb.draw(ctx, time, isEngineHyper, scale)
    })

    // 5. 绘制渊眚
    voidEyes.forEach(eye => eye.draw(ctx, w, h, mx, my, scale))

    time += isEngineHyper ? 0.08 : 0.03
    animationId = requestAnimationFrame(renderLoop)
  }
  renderLoop()
}

// --- 初始化与时钟 ---
onMounted(() => {
  bootTimeEngine()
  bootVoidEngine()
})

onBeforeUnmount(() => {
  clearInterval(timerInterval)
  cancelAnimationFrame(animationId)
})

let timerInterval
const bootTimeEngine = () => {
  const targetDate = new Date('2028-12-23T08:30:00').getTime()
  timerInterval = setInterval(() => {
    const diff = targetDate - Date.now()
    if (diff <= 0) { doomTimer.value = "JUDGEMENT_DAY"; return }
    const d = Math.floor(diff / 86400000).toString().padStart(3, '0')
    const h = Math.floor((diff / 3600000) % 24).toString().padStart(2, '0')
    const m = Math.floor((diff / 60000) % 60).toString().padStart(2, '0')
    const s = Math.floor((diff / 1000) % 60).toString().padStart(2, '0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001)
  }, 1000)
}
</script>

<style>
/* ============================================================================
   [卷六：流体自适应网格 (Responsive UI CSS Architecture)]
   核心：绝对隔离，面板玻璃化，允许底层 Canvas 透出。大小随布局完美自适应。
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=IM+Fell+English+SC&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  --c-ink: #111; --c-blood: #8B0000; --c-muted: #555; --c-glass: rgba(226, 222, 208, 0.65);
  --f-lore: 'IM Fell English SC', serif; --f-title: 'Cinzel', serif; --f-mono: 'Space Mono', monospace;
}

body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; background-color: #E2DED0; color: var(--c-ink); font-family: var(--f-lore); overflow: hidden; }

.nexus-root { position: relative; width: 100%; height: 100%; transition: filter 2s; }
.occult-svg-defs { position: absolute; pointer-events: none; }

/* 精神状态滤镜 */
.sanity-low { filter: url(#flesh-melt) contrast(1.1) saturate(1.2); }
.realm-hyper { filter: url(#void-glitch); }

.static-noise { position: fixed; inset: 0; z-index: 10; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="3"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.05; }
.void-scanlines { position: fixed; inset: 0; z-index: 11; pointer-events: none; background: linear-gradient(rgba(0,0,0,0) 50%, rgba(0,0,0,0.1) 50%); background-size: 100% 4px; }

.abyssal-canvas { position: absolute; inset: 0; z-index: 1; display: block; }

.text-crimson, .text-alert, .text-hyper { color: var(--c-blood); font-weight: bold; }
.text-hyper { animation: jitter 0.1s infinite; text-shadow: 0 0 10px red; }
.text-bone { color: #f2efe4; background: var(--c-ink); padding: 0 5px; }
.mt-auto { margin-top: auto; } .mt-4 { margin-top: 1.5rem; }

/* ----------------------------------------------------------------------------
   [响应式流体网格系统 (Fluid CSS Grid)]
   ---------------------------------------------------------------------------- */
.interface-grid {
  position: relative; z-index: 50; width: 100%; height: 100%; padding: 2vw 3vw; box-sizing: border-box;
  display: grid;
  /* 核心响应式：左右侧边栏最小250px，最大320px。中间留给神像和水池。 */
  grid-template-columns: minmax(250px, 320px) 1fr minmax(250px, 320px);
  grid-template-rows: auto 1fr auto;
  grid-template-areas: "header header header" "left center right" "footer footer footer";
  gap: 2vw; pointer-events: none;
}
.interface-grid > * { pointer-events: auto; }

/* 玻璃面板效应：让侧边栏不要遮挡底层的触手和墨水 */
.panel-glass { background: var(--c-glass); border: 1px solid rgba(0,0,0,0.1); backdrop-filter: blur(4px); padding: 1.5rem; border-radius: 8px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); }

/* --- 天极 --- */
.grid-header { grid-area: header; display: flex; justify-content: space-between; align-items: flex-end; border-bottom: 2px solid var(--c-ink); padding-bottom: 1rem; }
.title-construct { display: flex; flex-direction: column; }
.eldritch-title { font-family: var(--f-title); font-size: clamp(2rem, 4vw, 4rem); margin: 0; font-weight: 900; letter-spacing: 2px; }
.status-monitor { display: flex; align-items: center; gap: 8px; margin-top: 5px; opacity: 0.7; }
.pulse-sig { width: 8px; height: 8px; background-color: var(--c-blood); border-radius: 50%; animation: pulse 2s infinite; }
.mono-label { font-family: var(--f-mono); font-size: 0.75rem; font-weight: 700; letter-spacing: 1px; }

.chronos-construct { text-align: right; }
.doom-timer { font-size: clamp(1.8rem, 3vw, 3.5rem); font-weight: 700; font-family: var(--f-mono); letter-spacing: -2px; line-height: 1; }
.shake-timer { animation: jitter 0.5s infinite; color: var(--c-blood); }
.timer-label { font-size: 0.7rem; color: var(--c-muted); margin-top: 8px; letter-spacing: 2px; font-weight: bold; }

/* --- 左右侧边栏 --- */
.grid-left { grid-area: left; display: flex; flex-direction: column; }
.grid-right { grid-area: right; display: flex; flex-direction: column; }
.zone-title { font-size: 0.9rem; color: var(--c-muted); letter-spacing: 2px; border-bottom: 1px dashed rgba(0,0,0,0.3); padding-bottom: 8px; margin-bottom: 1rem; font-weight: bold; }

.pillar-matrix { display: flex; flex-direction: column; gap: 1rem; }
.pillar-node { display: flex; flex-wrap: wrap; align-items: center; gap: 10px; opacity: 0.5; cursor: pointer; transition: all 0.3s; }
.pillar-node:hover { opacity: 0.8; }
.pillar-node.active { opacity: 1; }
.pillar-node.active .node-icon { color: var(--c-paper); background: var(--c-ink); border-color: var(--c-ink); }
.pillar-node.active .node-name { color: var(--c-blood); font-weight: 900; }

.node-icon { width: 35px; height: 35px; border: 2px solid var(--c-muted); display: flex; align-items: center; justify-content: center; font-family: var(--f-title); font-size: 1rem; font-weight: 800; border-radius: 4px; }
.node-data { flex: 1; }
.node-name { font-weight: 700; font-size: 0.85rem; font-family: var(--f-mono); }
.node-status { font-size: 0.65rem; color: var(--c-muted); margin-top: 2px; font-family: var(--f-mono); }
.node-corruption-bar { width: 100%; height: 3px; background: rgba(0,0,0,0.1); margin-top: 6px; }
.bar-fill { height: 100%; background: var(--c-ink); transition: width 1s ease; }

.forge-stats, .metrics-matrix { font-family: var(--f-mono); font-size: 0.8rem; }
.forge-bar-wrap { width: 100%; height: 6px; background: rgba(0,0,0,0.1); border: 1px solid var(--c-ink); margin-top: 8px; }
.forge-bar, .forge-progress { height: 100%; } .forge-progress { background: var(--c-blood); }
.forge-dose-row { display: flex; justify-content: space-between; margin-top: 10px; }
.dose-val { font-size: 1.4rem; font-weight: 700; color: var(--c-ink); }

.metric-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid rgba(0,0,0,0.05); padding-bottom: 5px; margin-bottom: 8px; }
.metric-row .label { color: var(--c-muted); font-weight: bold; }
.metric-row .value { font-weight: 700; font-size: 1rem; }

.terminal-logs { flex: 1; border: 1px inset rgba(0,0,0,0.1); background: rgba(0,0,0,0.03); padding: 0.8rem; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; font-family: var(--f-mono); font-size: 0.65rem; line-height: 1.4; border-radius: 4px; min-height: 150px; }
.terminal-logs::-webkit-scrollbar { width: 4px; } .terminal-logs::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.2); }
.log-entry { display: flex; gap: 6px; word-break: break-all; }
.log-time { color: var(--c-muted); opacity: 0.7; flex-shrink: 0; }
.log-entry.info .log-msg { color: var(--c-ink); }
.log-entry.sys .log-msg { color: #555; }
.log-entry.success .log-msg { color: #006600; font-weight: bold; }
.log-entry.alert .log-msg, .log-entry.error .log-msg { color: var(--c-blood); }

/* --- 中枢：自适应神像与黑水池 --- */
.grid-center { grid-area: center; display: flex; justify-content: center; align-items: center; position: relative; min-width: 0; /* 关键：防止Flex子元素撑破容器 */ }
.altar-focus-zone { position: relative; width: 100%; height: 100%; display: flex; flex-direction: column; align-items: center; justify-content: flex-end; padding-bottom: 5%; }

/* 黑水容器：放置在底层耶稣神像(Canvas)之上 */
.abyssal-water-pool { position: absolute; bottom: 10%; width: min(30vw, 250px); height: min(40vh, 350px); z-index: 20; display: flex; align-items: flex-end; border-radius: 15px 15px 40px 40px; overflow: hidden; box-shadow: 0 20px 50px rgba(0,0,0,0.5); border: 2px solid rgba(26,26,26,0.8); }
.pool-glass-overlay { position: absolute; inset: 0; background: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 50%, rgba(0,0,0,0.4) 100%); pointer-events: none; z-index: 22; box-shadow: inset 0 0 20px rgba(0,0,0,0.8); }
.water-wave { width: 100%; background: #0A0A0A; position: relative; transition: height 1.5s cubic-bezier(0.1, 0.8, 0.3, 1); z-index: 21; }
/* 水波纹伪元素动画 */
.water-wave::before { content: ''; position: absolute; top: -10px; left: 0; width: 200%; height: 20px; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 20" xmlns="http://www.w3.org/2000/svg"><path d="M0 10 Q 25 20, 50 10 T 100 10 L 100 20 L 0 20 Z" fill="%230A0A0A"/></svg>'); background-size: 50% 100%; animation: wave-flow 3s linear infinite; }

@keyframes wave-flow { 0% { transform: translateX(0); } 100% { transform: translateX(-50%); } }

/* 进度读数悬浮在水池前 */
.siphon-readout { position: relative; z-index: 30; text-align: center; background: var(--c-glass); padding: 1.5rem 2rem; border-radius: 12px; box-shadow: 0 15px 35px rgba(0,0,0,0.3); border: 1px solid rgba(0,0,0,0.2); backdrop-filter: blur(8px); margin-bottom: -20px; }
.massive-percent .num { font-family: var(--f-title); font-size: clamp(4rem, 6vw, 7rem); font-weight: 900; line-height: 0.9; letter-spacing: -3px; color: var(--c-ink); }
.massive-percent .unit { font-size: clamp(1.5rem, 2vw, 2.5rem); color: var(--c-blood); font-family: var(--f-title); font-weight: 900; }
.vocab-total { font-family: var(--f-mono); font-size: 0.8rem; color: var(--c-muted); font-weight: bold; margin-top: 10px; }
.daily-sacrifice { font-family: var(--f-mono); font-size: 0.75rem; color: var(--c-blood); margin-top: 5px; font-weight: bold; animation: pulse 2s infinite; }

/* --- 地极 --- */
.grid-footer { grid-area: footer; display: flex; align-items: center; gap: 1rem; border: none; }
.cli-prefix { font-family: var(--f-mono); font-weight: 900; font-size: 1.2rem; color: var(--c-ink); }
.cli-input { flex: 1; background: transparent; border: none; outline: none; font-family: var(--f-lore); font-size: 1.5rem; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed rgba(0,0,0,0.3); padding-bottom: 5px; transition: border 0.3s; }
.cli-input:focus { border-bottom: 1px solid var(--c-blood); }
.cli-input::placeholder { color: var(--c-muted); opacity: 0.5; font-style: normal; font-family: var(--f-mono); font-size: 0.9rem; }

/* 死亡回归覆盖层 */
.death-overlay { position: fixed; inset: 0; z-index: 9000; background: #050505; display: flex; justify-content: center; align-items: center; flex-direction: column; }
.glitch-word { font-family: var(--f-title); font-size: clamp(4rem, 10vw, 8rem); color: #FFF; line-height: 0.9; animation: jitter 0.1s infinite; }
.sub-death { font-family: var(--f-mono); color: var(--c-muted); margin-top: 2rem; letter-spacing: 5px; animation: pulse 1s infinite; }

@keyframes pulse { 0% { opacity: 0.3; } 50% { opacity: 1; } 100% { opacity: 0.3; } }
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }

/* ----------------------------------------------------------------------------
   [极端降维防御 (Responsive Collapsing)]
   当屏幕极小(如手机)时，网格坍缩为单列，但保证不重叠。
   ---------------------------------------------------------------------------- */
@media (max-width: 1100px) {
  .interface-grid {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto auto auto;
    grid-template-areas: "header" "center" "left" "right" "footer";
    overflow-y: auto; height: 100%; pointer-events: auto;
  }
  .grid-left, .grid-right { max-width: 500px; margin: 0 auto; width: 100%; }
  .grid-center { height: 60vh; min-height: 500px; }
  .abyssal-water-pool { bottom: 5%; width: 200px; height: 300px; }
}
</style>