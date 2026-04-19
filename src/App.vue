/* [阿卡姆飞升魔典 V6.66 // 终极因果律重塑版]
   [核心法则：底层 Canvas 灵魂映射 | 纯粹神秘学驱动 | 絕對隔離排版]
   [契约者：武少康 // 目标：22408 飞升]
   [警告：此魔典已逾千行，逻辑极度交织，严禁随意删改刻印。]
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
          <feGaussianBlur in="SourceGraphic" stdDeviation="2" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="melt" />
          <feBlend in="SourceGraphic" in2="melt" />
        </filter>
      </defs>
    </svg>

    <div class="static-noise"></div>
    <div class="void-scanlines"></div>
    <div class="blood-vignette"></div>

    <div class="interface-grid">
      
      <header class="top-sect">
        <div class="title-construct">
          <h1 class="eldritch-title" :data-text="pageTitle">{{ pageTitle }}</h1>
          <div class="status-monitor">
            <span class="pulse-sig"></span>
            <span class="mono-label">GRIMOIRE_V6.66 // ABYSSAL_ALTAR // [ {{ systemStatus }} ]</span>
          </div>
        </div>
        <div class="chronos-construct">
          <div class="doom-timer" :class="{'shake-timer': karmaLoad > 80}">{{ doomTimer }}</div>
          <div class="timer-label">UNTIL_THE_STARS_ARE_RIGHT (2028-12)</div>
        </div>
      </header>

      <aside class="left-pillar-sect">
        <h2 class="zone-title">THE_PILLARS_OF_KNOWLEDGE</h2>
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
              <div class="node-name">VOID (ADVANCED_MATH)</div>
              <div class="node-status">{{ getPillarStatus('MATH') }}</div>
            </div>
            <div class="node-corruption-bar"><div class="bar-fill" :style="{width: pillarProgress.MATH + '%'}"></div></div>
          </div>
        </div>
        
        <div class="forge-stats mt-auto">
          <h2 class="zone-title">FLESH_ALCHEMY (GYM)</h2>
          <div class="forge-data">
            <div class="forge-label">CYCLE_RITUAL: 3-ON / 2-OFF</div>
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

      <main class="center-rift-sect">
        <div class="rift-containment" :class="{'rift-unstable': isSanityBroken}">
          <canvas ref="mainCanvas" class="main-canvas"></canvas>
          
          <div class="siphon-readout" :class="{'text-melt': sanityIndex < 20}">
            <div class="readout-header">GRIMOIRE_TRANSLATION_PROGRESS</div>
            <div class="massive-percent">
              <span class="num">{{ vocabProgressPercentage }}</span>
              <span class="unit">%</span>
            </div>
            <div class="vocab-total">{{ vocabSiphoned }} / 4000 SOULS_SIPHONED</div>
            <div class="daily-sacrifice">TODAY_SACRIFICE: +{{ dailyVocab }}</div>
          </div>
        </div>
      </main>

      <aside class="right-metrics-sect">
        <h2 class="zone-title">SOUL_VIBRATION_METRICS</h2>
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
          <div class="metric-row">
            <span class="label">GAZE_OF_SENIOR:</span>
            <span class="value text-glitch" :class="{'active': isGazed}">{{ isGazed ? 'PIERCING' : 'VEILED' }}</span>
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

      <footer class="input-sect">
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

    <div class="grimoire-modal" v-if="showGrimoire">
      <div class="modal-content">
        <h3 class="modal-title">FORBIDDEN_KNOWLEDGE: {{ activePillar }}</h3>
        <ul class="knowledge-list">
          <li v-for="(spell, idx) in currentGrimoireSpells" :key="idx" class="spell-item">
            <span class="spell-rune">☨</span> {{ spell }}
          </li>
        </ul>
        <button class="close-btn" @click="showGrimoire = false">SEAL_THE_BOOK</button>
      </div>
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
// [卷一：灵魂刻印与持久化法则 (Persistence & State)]
// ============================================================================

const loadEngram = (k, def) => Number(localStorage.getItem(`vzuor_v6_${k}`)) || def
const saveEngram = (k, val) => localStorage.setItem(`vzuor_v6_${k}`, val)

const doomTimer = ref('000D | 00:00:00')
const vocabSiphoned = ref(loadEngram('vocab', 2150))
const dailyVocab = ref(loadEngram('daily_vocab', 0))
const karmaLoad = ref(loadEngram('karma', 23.7))
const caffeineLvl = ref(200)
const sanityIndex = ref(loadEngram('sanity', 99.9999))
const deathReturns = ref(loadEngram('deaths', 0))

const activePillar = ref('C_LANG')
const isGazed = ref(false)
const showGrimoire = ref(false)

const pillarProgress = reactive({
  C_LANG: loadEngram('prog_c', 35),
  DATA_STRUCT: loadEngram('prog_ds', 12),
  MATH: loadEngram('prog_math', 5)
})

const vocabProgressPercentage = computed(() => {
  return Math.floor((vocabSiphoned.value / 4000) * 100)
})

const sanityStageClass = computed(() => {
  if (sanityIndex.value > 80) return 'sanity-high'
  if (sanityIndex.value > 40) return 'sanity-med'
  return 'sanity-low'
})

const pageTitle = ref("V'ZUOR KHAA-SH'AN")
const systemStatus = ref('SOUL_BINDING_ACTIVE')
const realmState = ref('realm-stable')
const isDeathReturning = ref(false)
const isSanityBroken = ref(false)

const currentCmd = ref('')
const commandHistoryRing = []
let historyIndex = -1

const cmdHistory = reactive([
  { time: getSysTime(), type: 'sys', msg: '阿卡姆底层协议 V6.66 觉醒。' },
  { time: getSysTime(), type: 'sys', msg: '因果律防线已部署。22408 飞升矩阵锚定完成。' },
  { time: getSysTime(), type: 'alert', msg: '警告：检测到大量不可视之物的窥探。' }
])
const logBox = ref(null)

function getSysTime() {
  const d = new Date()
  return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}.${d.getMilliseconds().toString().padStart(3,'0')}`
}

// ============================================================================
// [卷二：22408 禁忌法典库 (The Grimoire Syllabus)]
// ============================================================================
const forbiddenKnowledge = {
  C_LANG: [
    "POINTER_ALCHEMY: 操控虚空内存的指针之术",
    "STRUCT_MOLDING: 编织血肉的结构体定义",
    "MACRO_INCANTATION: 预处理器的宏观低语",
    "MEMORY_LEAK_CURSE: 游离于虚空的内存反噬",
    "ARRAY_DECAY: 连续空间的腐朽与退化"
  ],
  DATA_STRUCT: [
    "NON_EUCLIDEAN_GRAPHS: 非欧几里得图论遍历",
    "ABYSSAL_TREES: 扎根虚空的二叉搜索树",
    "LINKED_VEINS: 链接血肉的单/双向链表",
    "DYNAMIC_PROPHECY: 洞悉未来的动态规划",
    "HASH_MAPPING: 灵魂印记的哈希映射"
  ],
  MATH: [
    "LIMIT_OF_SANITY: 逼近疯狂的极限推导",
    "DERIVATIVE_TORTURE: 撕裂曲线的微积分",
    "VOID_INTEGRALS: 吞噬面积的多重积分",
    "INFINITE_SERIES: 无尽轮回的泰勒展开"
  ]
}

const currentGrimoireSpells = computed(() => forbiddenKnowledge[activePillar.value])

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

// ============================================================================
// [卷三：宿命时钟与理智流失 (Chronos & Sanity Engine)]
// ============================================================================
let timerInterval
const bootTimeEngine = () => {
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
    
    // 理智值(SAN) 根据业障(Karma)流失
    const drainRate = 0.0001 + (karmaLoad.value * 0.00001)
    sanityIndex.value = Math.max(0, sanityIndex.value - drainRate)
    
    // 随机触发高维凝视
    if (Math.random() < 0.005 && !isGazed.value) triggerGaze()
  }, 1000)
}

// ============================================================================
// [卷四：仪式指令解析器 (Ritual CLI Parser)]
// ============================================================================
const pushLog = (msg, type = 'info') => {
  cmdHistory.push({ time: getSysTime(), type, msg })
  if (cmdHistory.length > 100) cmdHistory.shift()
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
      pushLog(' - read          - 翻开禁忌法典 (查看当前柱神知识)', 'sys')
      pushLog(' - study [num]   - 献祭理智提升当前柱神进度', 'sys')
      pushLog(' - cleanse       - 消耗词汇量洗刷业障', 'sys')
      pushLog(' - vzuor         - [禁忌] 呼唤不可名状之真名', 'alert')
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
        
        pushLog(`灵魂已剥夺。祭坛水位上升。进度: <span class="text-bone">${vocabSiphoned.value}/4000</span>`, 'success')
      }
      break

    case 'dose':
      const dose = parseInt(args[1])
      if (isNaN(dose)) {
        pushLog('劣质的灵药。', 'error')
      } else {
        caffeineLvl.value = dose
        if (dose >= 400) {
          triggerHyperDrive()
        } else {
          pushLog(`灵药注入。肉身机能锁定在 ${dose}mg 阈值。`, 'info')
        }
      }
      break

    case 'focus':
      const p = args[1]
      const pillars = {'c':'C_LANG', 'data':'DATA_STRUCT', 'math':'MATH'}
      if (pillars[p]) {
        focusPillar(pillars[p])
      } else {
        pushLog('未知的虚空坐标。使用 chant 查看。', 'error')
      }
      break

    case 'read':
      showGrimoire.value = true
      pushLog(`翻开了关于 [${activePillar.value}] 的残卷。`, 'sys')
      break

    case 'study':
      const stAmt = parseInt(args[1])
      if (isNaN(stAmt) || stAmt <= 0) {
        pushLog('缺乏专注，研究失败。', 'error')
      } else {
        if (sanityIndex.value < stAmt * 0.5) {
          pushLog('理智不足，强行研究将导致灵魂撕裂。', 'alert')
        } else {
          sanityIndex.value -= stAmt * 0.5
          pillarProgress[activePillar.value] = Math.min(100, pillarProgress[activePillar.value] + stAmt)
          saveEngram(`prog_${activePillar.value.toLowerCase().split('_')[0]}`, pillarProgress[activePillar.value])
          pushLog(`消耗了理智。对 [${activePillar.value}] 的理解加深了 ${stAmt}%。`, 'success')
        }
      }
      break

    case 'cleanse':
      if (vocabSiphoned.value >= 100) {
        vocabSiphoned.value -= 100
        karmaLoad.value = Math.max(0, karmaLoad.value - 15)
        pushLog('献祭了 100 个灵魂碎片。业障被洗刷。', 'success')
      } else {
        pushLog('灵魂碎片不足，无法取悦深渊。', 'error')
      }
      break

    case 'vzuor':
      triggerSanityBreak()
      break

    case 'die':
      triggerDeathReturn()
      break

    default:
      karmaLoad.value += 1.5
      saveEngram('karma', karmaLoad.value)
      pushLog(`呢喃着无意义的音节。业障增加。`, 'error')
  }
  currentCmd.value = ''
}

// --- 极端状态与神秘学仪式触发 ---
const triggerHyperDrive = () => {
  realmState.value = 'realm-hyper'
  pushLog('警告：灵药过载！血肉正在溶解，灵魂频率飙升...', 'error')
  if(window.vzuorEngine) window.vzuorEngine.setHyper(true)
  
  setTimeout(() => {
    realmState.value = 'realm-stable'
    caffeineLvl.value = 200
    if(window.vzuorEngine) window.vzuorEngine.setHyper(false)
    pushLog('灵药反噬结束。理智轻微受损。', 'sys')
    sanityIndex.value -= 2.0
  }, 8000)
}

const triggerGaze = () => {
  isGazed.value = true
  pushLog('<span class="text-glitch">虚空中的高维实体正在凝视你。</span>', 'alert')
  setTimeout(() => isGazed.value = false, 6000)
}

const triggerSanityBreak = () => {
  isSanityBroken.value = true
  pushLog('<span class="text-crimson">真名回响。不可名状之物撕裂了现实防线。</span>', 'alert')
  sanityIndex.value -= 30.0
  setTimeout(() => {
    isSanityBroken.value = false
    pushLog('防线重组。你遗忘了刚才看到的东西。', 'sys')
  }, 5000)
}

const triggerDeathReturn = () => {
  isDeathReturning.value = true
  deathReturns.value += 1
  saveEngram('deaths', deathReturns.value)
  karmaLoad.value += 20.0
  sanityIndex.value = 100.0 // 重置理智，但保留业障
  pushLog(`执行灵魂剥离。启动第 ${deathReturns.value} 次因果重置。`, 'error')
  
  setTimeout(() => {
    isDeathReturning.value = false
    pushLog('世界线已缝合。带着新的业障继续前行。', 'sys')
  }, 4000)
}


// ============================================================================
// [卷五：虚空引擎 (The Abyssal Engine) - 灵魂映射与神秘学几何]
// 核心逻辑：摒弃外部图片，全代码手绘神秘学符文、触手与不可视之眼
// ============================================================================
const mainCanvas = ref(null)

// 1. 神秘学几何肢体 (Occult Geometry Limbs)
class ElderLimb {
  constructor(x, y, segments, length, color) {
    this.baseX = x; this.baseY = y
    this.segments = segments; this.segLength = length
    this.joints = []
    for(let i=0; i<segments; i++) this.joints.push({x: x, y: y + i * length})
    this.color = color
    this.noiseOffset = Math.random() * 1000
  }

  manifest(targetX, targetY) {
    this.joints[this.segments-1] = { x: targetX, y: targetY }
    for(let i = this.segments - 2; i >= 0; i--) {
      const dx = this.joints[i].x - this.joints[i+1].x
      const dy = this.joints[i].y - this.joints[i+1].y
      const dist = Math.sqrt(dx*dx + dy*dy)
      const ratio = this.segLength / (dist || 1)
      this.joints[i].x = this.joints[i+1].x + dx * ratio
      this.joints[i].y = this.joints[i+1].y + dy * ratio
    }
    this.joints[0] = { x: this.baseX, y: this.baseY }
    for(let i = 1; i < this.segments; i++) {
      const dx = this.joints[i].x - this.joints[i-1].x
      const dy = this.joints[i].y - this.joints[i-1].y
      const dist = Math.sqrt(dx*dx + dy*dy)
      const ratio = this.segLength / (dist || 1)
      this.joints[i].x = this.joints[i-1].x + dx * ratio
      this.joints[i].y = this.joints[i-1].y + dy * ratio
    }
  }

  draw(ctx, time, isHyper) {
    ctx.beginPath()
    ctx.moveTo(this.joints[0].x, this.joints[0].y)
    for (let i = 1; i < this.segments; i++) {
      const wob = Math.sin(time * (isHyper?4:1) + i * 0.4 + this.noiseOffset) * (isHyper?4:1.5)
      const dx = this.joints[i].x - this.joints[i-1].x
      const dy = this.joints[i].y - this.joints[i-1].y
      const angle = Math.atan2(dy, dx)
      const px = this.joints[i].x + Math.sin(angle) * wob
      const py = this.joints[i].y - Math.cos(angle) * wob
      ctx.lineTo(px, py)
    }
    ctx.strokeStyle = this.color
    ctx.lineWidth = isHyper ? 4 : 2
    ctx.lineCap = 'round'
    ctx.stroke()
  }
}

// 2. 业障灰烬 (Karma Ash)
class KarmaAsh {
  constructor(w, h) {
    this.w = w; this.h = h; this.reset()
  }
  reset() {
    this.x = (this.w / 2) + (Math.random() - 0.5) * 800
    this.y = this.h + 10
    this.vx = (Math.random() - 0.5) * 2
    this.vy = -Math.random() * 3 - 1
    this.life = Math.random() * 150 + 50
    this.maxLife = this.life
    this.size = Math.random() * 2.5 + 0.5
  }
  update(isHyper, riftX, riftY) {
    this.x += this.vx * (isHyper ? 2 : 1)
    this.y += this.vy * (isHyper ? 2 : 1)
    
    // 灵魂被中央裂隙吸引
    const dx = riftX - this.x
    const dy = riftY - this.y
    const dist = Math.sqrt(dx*dx + dy*dy)
    if (dist < 300) {
      this.vx += dx * 0.002
      this.vy += dy * 0.002
    }

    this.life--
    if (this.life <= 0 || this.y < -10) this.reset()
  }
  draw(ctx, isHyper) {
    const op = (this.life / this.maxLife) * 0.8
    ctx.fillStyle = isHyper ? `rgba(163, 29, 29, ${op})` : `rgba(20, 25, 20, ${op})`
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2)
    ctx.fill()
  }
}

// 3. 渊眚 (Void Eye)
class VoidEye {
  constructor(x, y, radius) {
    this.baseX = x; this.baseY = y; this.r = radius
    this.pupilX = x; this.pupilY = y
    this.blinkTimer = Math.random() * 100
  }
  update(mx, my) {
    const dx = mx - this.baseX
    const dy = my - this.baseY
    const dist = Math.sqrt(dx*dx + dy*dy)
    const maxPupilOffset = this.r * 0.5
    
    if (dist > 0) {
      this.pupilX = this.baseX + (dx / dist) * Math.min(dist * 0.1, maxPupilOffset)
      this.pupilY = this.baseY + (dy / dist) * Math.min(dist * 0.1, maxPupilOffset)
    }
    this.blinkTimer--
    if (this.blinkTimer < -5) this.blinkTimer = Math.random() * 200 + 50
  }
  draw(ctx) {
    ctx.save()
    // 绘制眼白
    ctx.beginPath()
    ctx.arc(this.baseX, this.baseY, this.r, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(230, 220, 210, 0.8)'
    ctx.fill()
    ctx.lineWidth = 2
    ctx.strokeStyle = 'rgba(20, 20, 20, 0.9)'
    ctx.stroke()
    
    // 绘制瞳孔
    if (this.blinkTimer > 0) {
      ctx.beginPath()
      ctx.arc(this.pupilX, this.pupilY, this.r * 0.4, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(10, 5, 5, 0.95)'
      ctx.fill()
      
      // 诡异红血丝
      ctx.beginPath()
      ctx.arc(this.pupilX, this.pupilY, this.r * 0.15, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(180, 20, 20, 0.8)'
      ctx.fill()
    } else {
      // 闭眼状态
      ctx.beginPath()
      ctx.moveTo(this.baseX - this.r, this.baseY)
      ctx.lineTo(this.baseX + this.r, this.baseY)
      ctx.lineWidth = 3
      ctx.stroke()
    }
    ctx.restore()
  }
}

let animationId, ctx, w, h, time = 0
let elderLimbs = [], karmaAshes = [], voidEyes = []
let isEngineHyper = false

const bootVoidEngine = () => {
  if (!mainCanvas.value) return
  ctx = mainCanvas.value.getContext('2d')
  w = mainCanvas.value.clientWidth; h = mainCanvas.value.clientHeight
  mainCanvas.value.width = w; mainCanvas.value.height = h

  // 孕育不可视之物
  for (let i = 0; i < 12; i++) {
    elderLimbs.push(new ElderLimb(w/2, h/2, 15, 20, 'rgba(15, 18, 15, 0.5)'))
  }
  for (let i = 0; i < 150; i++) karmaAshes.push(new KarmaAsh(w, h))
  
  // 散布渊眚
  voidEyes.push(new VoidEye(w*0.2, h*0.3, 15))
  voidEyes.push(new VoidEye(w*0.8, h*0.2, 20))
  voidEyes.push(new VoidEye(w*0.75, h*0.7, 12))

  let mx = w/2, my = h/2
  window.addEventListener('mousemove', (e) => {
    const rect = mainCanvas.value.getBoundingClientRect()
    mx = e.clientX - rect.left; my = e.clientY - rect.top
  })
  window.addEventListener('resize', () => {
    w = mainCanvas.value.clientWidth; h = mainCanvas.value.clientHeight
    mainCanvas.value.width = w; mainCanvas.value.height = h
    elderLimbs.forEach(l => { l.baseX = w/2; l.baseY = h/2 })
  })

  window.vzuorEngine = { setHyper: (val) => isEngineHyper = val }

  const renderLoop = () => {
    // 灵魂拖影清理
    ctx.fillStyle = isEngineHyper ? 'rgba(50, 5, 5, 0.2)' : 'rgba(226, 222, 208, 0.25)'
    ctx.fillRect(0, 0, w, h)

    // 中心裂隙绘制 (The Rift)
    const riftRadius = 150 + Math.sin(time) * 10
    ctx.beginPath()
    ctx.arc(w/2, h/2, riftRadius, 0, Math.PI * 2)
    const grad = ctx.createRadialGradient(w/2, h/2, 10, w/2, h/2, riftRadius)
    grad.addColorStop(0, isEngineHyper ? 'rgba(150, 0, 0, 0.8)' : 'rgba(10, 20, 15, 0.9)')
    grad.addColorStop(1, 'rgba(226, 222, 208, 0)')
    ctx.fillStyle = grad
    ctx.fill()

    // 绘制业障灰烬
    karmaAshes.forEach(p => { p.update(isEngineHyper, w/2, h/2); p.draw(ctx, isEngineHyper) })

    // 绘制远古肢体 (触手)
    elderLimbs.forEach((limb, idx) => {
      // 狂化时剧烈扭动，平时一半追踪神识，一半自由游走
      let tx, ty
      if (isEngineHyper) {
        tx = w/2 + Math.cos(time*2 + idx) * 300
        ty = h/2 + Math.sin(time*3 + idx) * 300
      } else {
        tx = idx % 2 === 0 ? mx : w/2 + Math.cos(time*0.5 + idx) * 200
        ty = idx % 2 === 0 ? my : h/2 + Math.sin(time*0.6 + idx) * 200
      }
      limb.manifest(tx, ty)
      limb.color = isEngineHyper ? 'rgba(100, 10, 10, 0.7)' : 'rgba(15, 18, 15, 0.5)'
      limb.draw(ctx, time, isEngineHyper)
    })

    // 绘制渊眚
    voidEyes.forEach(eye => {
      eye.update(mx, my)
      eye.draw(ctx)
    })

    time += isEngineHyper ? 0.08 : 0.02
    animationId = requestAnimationFrame(renderLoop)
  }
  renderLoop()
}

// --- 初始化序列 ---
onMounted(() => {
  bootTimeEngine()
  setTimeout(bootVoidEngine, 100)
})

onBeforeUnmount(() => {
  clearInterval(timerInterval)
  cancelAnimationFrame(animationId)
})

</script>

<style>
/* ============================================================================
   [卷六：绝界织物 (CSS Architecture)]
   核心：绝对不重叠的网格，极其复杂的关键帧，阴间滤镜。
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=IM+Fell+English+SC&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  --c-void: #0A0A0A;
  --c-paper: #E2DED0;
  --c-ink: #181A18;
  --c-blood: #8B0000;
  --c-terminal: #00FF66;
  --c-muted: #55534A;
  
  --f-lore: 'IM Fell English SC', serif;
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', monospace;
}

body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; background-color: var(--c-paper); color: var(--c-ink); font-family: var(--f-lore); overflow: hidden; }

.nexus-root { position: relative; width: 100%; height: 100%; transition: filter 2s; }
.occult-svg-defs { position: absolute; pointer-events: none; }

/* 精神状态反馈层 */
.sanity-low { filter: url(#flesh-melt) contrast(1.2) sepia(0.3); }
.realm-hyper { filter: url(#void-glitch); }

/* 背景噪音 */
.static-noise { position: fixed; inset: 0; z-index: 10; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="3"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.05; }
.void-scanlines { position: fixed; inset: 0; z-index: 11; pointer-events: none; background: linear-gradient(rgba(0,0,0,0) 50%, rgba(0,0,0,0.1) 50%); background-size: 100% 4px; }
.blood-vignette { position: fixed; inset: 0; z-index: 12; pointer-events: none; background: radial-gradient(circle at center, transparent 40%, rgba(50, 0, 0, 0.2) 100%); }

/* 文本辅助 */
.text-crimson { color: var(--c-blood); }
.text-alert { color: #A31D1D; font-weight: bold; }
.text-hyper { color: #FF0000; font-weight: 900; animation: jitter 0.1s infinite; text-shadow: 0 0 10px red; }
.text-bone { color: #f2efe4; font-weight: 700; background: var(--c-ink); padding: 0 5px; }
.text-glitch { animation: slight-glitch 2s infinite; }
.text-melt { animation: melt-drop 3s infinite; }
.mt-auto { margin-top: auto; }
.mt-4 { margin-top: 1.5rem; }

/* ----------------------------------------------------------------------------
   [绝对网格隔离系统]
   ---------------------------------------------------------------------------- */
.interface-grid {
  position: relative; z-index: 50; width: 100%; height: 100%; padding: 2.5rem 3rem; box-sizing: border-box;
  display: grid; grid-template-columns: 320px 1fr 340px; grid-template-rows: auto 1fr auto;
  grid-template-areas: "header header header" "left center right" "footer footer footer";
  gap: 2rem; pointer-events: none;
}
.interface-grid > * { pointer-events: auto; }

/* --- 天极 --- */
.top-sect { grid-area: header; display: flex; justify-content: space-between; align-items: flex-end; border-bottom: 2px solid var(--c-ink); padding-bottom: 1rem; }
.title-construct { display: flex; flex-direction: column; }
.eldritch-title { font-family: var(--f-title); font-size: clamp(2.5rem, 3.5vw, 4rem); margin: 0; font-weight: 900; letter-spacing: 2px; position: relative; }
/* 标题双重叠影 */
.eldritch-title::before { content: attr(data-text); position: absolute; left: -2px; text-shadow: 2px 0 red; top: 0; opacity: 0.5; z-index: -1; animation: glitch-anim-1 3s infinite linear alternate-reverse; }
.eldritch-title::after { content: attr(data-text); position: absolute; left: 2px; text-shadow: -2px 0 blue; top: 0; opacity: 0.5; z-index: -1; animation: glitch-anim-2 2s infinite linear alternate-reverse; }

.status-monitor { display: flex; align-items: center; gap: 8px; margin-top: 5px; opacity: 0.7; }
.pulse-sig { width: 8px; height: 8px; background-color: var(--c-blood); border-radius: 50%; animation: pulse 2s infinite; }
.mono-label { font-family: var(--f-mono); font-size: 0.75rem; font-weight: 700; letter-spacing: 1px; }

.chronos-construct { text-align: right; }
.doom-timer { font-size: clamp(2rem, 3vw, 3.5rem); font-weight: 700; font-family: var(--f-mono); letter-spacing: -2px; line-height: 1; }
.shake-timer { animation: jitter 0.5s infinite; color: var(--c-blood); }
.timer-label { font-size: 0.7rem; color: var(--c-muted); margin-top: 8px; letter-spacing: 2px; font-weight: bold; }

/* --- 左渊 --- */
.left-pillar-sect { grid-area: left; display: flex; flex-direction: column; border-right: 1px dashed var(--c-muted); padding-right: 2rem; }
.zone-title { font-size: 0.95rem; color: var(--c-muted); letter-spacing: 3px; border-bottom: 1px solid rgba(0,0,0,0.2); padding-bottom: 8px; margin-bottom: 1.5rem; font-weight: bold; }

.pillar-matrix { display: flex; flex-direction: column; gap: 1.5rem; }
.pillar-node { display: flex; flex-wrap: wrap; align-items: center; gap: 1rem; opacity: 0.4; transition: all 0.3s; cursor: pointer; }
.pillar-node:hover { opacity: 0.8; }
.pillar-node.active { opacity: 1; }
.pillar-node.active .node-icon { color: var(--c-paper); background: var(--c-ink); border-color: var(--c-ink); }
.pillar-node.active .node-name { color: var(--c-blood); font-weight: 900; }

.node-icon { width: 40px; height: 40px; border: 2px solid var(--c-muted); display: flex; align-items: center; justify-content: center; font-family: var(--f-title); font-size: 1.2rem; font-weight: 800; transition: all 0.3s; }
.node-data { flex: 1; }
.node-name { font-weight: 700; font-size: 0.9rem; font-family: var(--f-mono); letter-spacing: -0.5px; }
.node-status { font-size: 0.65rem; color: var(--c-muted); margin-top: 3px; font-family: var(--f-mono); }
.node-corruption-bar { width: 100%; height: 3px; background: rgba(0,0,0,0.1); margin-top: 8px; }
.bar-fill { height: 100%; background: var(--c-ink); transition: width 1s ease; }

.forge-stats { font-family: var(--f-mono); }
.forge-bar-wrap { width: 100%; height: 6px; background: rgba(0,0,0,0.1); border: 1px solid var(--c-ink); margin-top: 8px; }
.forge-bar { height: 100%; width: 100%; }
.forge-progress { height: 100%; background: var(--c-blood); }
.forge-dose-row { display: flex; justify-content: space-between; align-items: center; margin-top: 12px; font-size: 0.8rem; }
.dose-val { font-size: 1.6rem; font-weight: 700; color: var(--c-ink); }

/* --- 中枢 --- */
.center-rift-sect { grid-area: center; display: flex; justify-content: center; align-items: center; position: relative; }
.rift-containment { position: absolute; inset: 0; z-index: 10; display: flex; justify-content: center; align-items: center; border-radius: 50%; /* 裂隙感 */ }
.rift-unstable { animation: throb 0.5s infinite alternate; }
.main-canvas { width: 100%; height: 100%; display: block; mix-blend-mode: multiply; }

.siphon-readout { position: absolute; z-index: 100; text-align: center; pointer-events: none; background: rgba(226, 222, 208, 0.85); padding: 2rem; border-radius: 50%; box-shadow: 0 0 50px rgba(0,0,0,0.2); border: 1px dashed rgba(0,0,0,0.3); backdrop-filter: blur(5px); }
.readout-header { font-size: 0.75rem; color: var(--c-muted); letter-spacing: 4px; font-weight: bold; font-family: var(--f-mono); }
.massive-percent { margin: 10px 0; }
.massive-percent .num { font-family: var(--f-title); font-size: 9rem; font-weight: 900; line-height: 0.85; letter-spacing: -5px; color: var(--c-ink); }
.massive-percent .unit { font-size: 2.5rem; color: var(--c-blood); vertical-align: top; font-family: var(--f-title); }
.vocab-total { font-family: var(--f-mono); font-size: 0.8rem; color: var(--c-muted); font-weight: bold; }
.daily-sacrifice { font-family: var(--f-mono); font-size: 0.7rem; color: var(--c-blood); margin-top: 5px; }

/* --- 右翼 --- */
.right-metrics-sect { grid-area: right; display: flex; flex-direction: column; border-left: 1px dashed var(--c-muted); padding-left: 2rem; }
.metrics-matrix { display: flex; flex-direction: column; gap: 15px; font-size: 0.85rem; font-family: var(--f-mono); }
.metric-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid rgba(0,0,0,0.05); padding-bottom: 5px; }
.metric-row .label { color: var(--c-muted); font-weight: bold; }
.metric-row .value { font-weight: 700; color: var(--c-ink); }

.terminal-logs { flex: 1; border: 1px solid rgba(0,0,0,0.2); background: rgba(0,0,0,0.02); padding: 1rem; overflow-y: auto; display: flex; flex-direction: column; gap: 8px; font-family: var(--f-mono); font-size: 0.7rem; line-height: 1.5; box-shadow: inset 0 0 20px rgba(0,0,0,0.05); }
.terminal-logs::-webkit-scrollbar { width: 4px; }
.terminal-logs::-webkit-scrollbar-thumb { background: var(--c-muted); }
.log-entry { display: flex; gap: 10px; word-break: break-all; }
.log-time { color: var(--c-muted); opacity: 0.7; flex-shrink: 0; }
.log-entry.info .log-msg { color: var(--c-ink); }
.log-entry.sys .log-msg { color: #444; }
.log-entry.echo .log-msg { color: #888; font-style: italic; }
.log-entry.success .log-msg { color: #005500; font-weight: bold; }
.log-entry.alert .log-msg, .log-entry.error .log-msg { color: var(--c-blood); }

/* --- 地极 --- */
.input-sect { grid-area: footer; border-top: 2px solid var(--c-ink); padding-top: 1.5rem; }
.input-cli { display: flex; align-items: center; gap: 1.5rem; }
.cli-prefix { font-family: var(--f-mono); font-weight: 900; font-size: 1.4rem; color: var(--c-ink); }
.cli-input { flex: 1; background: transparent; border: none; outline: none; font-family: var(--f-lore); font-size: 1.8rem; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed rgba(0,0,0,0.2); padding-bottom: 5px; transition: border 0.3s; }
.cli-input:focus { border-bottom: 1px solid var(--c-blood); }
.cli-input::placeholder { color: var(--c-muted); opacity: 0.5; font-style: normal; font-family: var(--f-mono); font-size: 0.9rem; }
.cli-input:disabled { opacity: 0.2; cursor: not-allowed; }

/* ----------------------------------------------------------------------------
   [模态与覆盖层：魔法书与死亡回归]
   ---------------------------------------------------------------------------- */
.grimoire-modal { position: fixed; inset: 0; z-index: 2000; background: rgba(10, 10, 10, 0.8); backdrop-filter: blur(5px); display: flex; justify-content: center; align-items: center; }
.modal-content { background: var(--c-paper); border: 2px solid var(--c-ink); padding: 3rem; width: 600px; max-width: 90%; box-shadow: 0 0 50px rgba(0,0,0,0.8); position: relative; }
.modal-content::before { content: ''; position: absolute; inset: 5px; border: 1px dashed var(--c-muted); pointer-events: none; }
.modal-title { font-family: var(--f-title); font-size: 1.5rem; color: var(--c-blood); text-align: center; margin-top: 0; border-bottom: 1px solid var(--c-muted); padding-bottom: 1rem; }
.knowledge-list { list-style: none; padding: 0; margin: 2rem 0; font-family: var(--f-mono); font-size: 0.9rem; line-height: 1.8; }
.spell-item { border-bottom: 1px dashed rgba(0,0,0,0.1); padding: 8px 0; display: flex; align-items: flex-start; gap: 10px; }
.spell-rune { color: var(--c-blood); font-size: 1.2rem; }
.close-btn { display: block; width: 100%; padding: 1rem; background: var(--c-ink); color: var(--c-paper); border: none; font-family: var(--f-title); font-size: 1.2rem; cursor: pointer; transition: background 0.3s; }
.close-btn:hover { background: var(--c-blood); }

.death-overlay { position: fixed; inset: 0; z-index: 9000; background: #050505; display: flex; justify-content: center; align-items: center; flex-direction: column; }
.death-text-container { text-align: center; }
.glitch-word { font-family: var(--f-title); font-size: 8rem; color: #FFF; line-height: 0.9; animation: violent-shake 0.1s infinite; }
.sub-death { font-family: var(--f-mono); color: var(--c-muted); margin-top: 2rem; letter-spacing: 5px; animation: pulse 1s infinite; }

/* ----------------------------------------------------------------------------
   [阿卡姆关键帧动画库]
   ---------------------------------------------------------------------------- */
@keyframes pulse { 0% { opacity: 0.3; } 50% { opacity: 1; } 100% { opacity: 0.3; } }
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes violent-shake { 0% { transform: translate(3px, 3px) rotate(1deg); } 25% { transform: translate(-3px, -2px) rotate(-1deg); } 50% { transform: translate(-2px, 3px) rotate(0deg); } 75% { transform: translate(3px, -3px) rotate(1deg); } 100% { transform: translate(-1px, 2px) rotate(-1deg); } }
@keyframes throb { 0% { transform: scale(1); } 100% { transform: scale(1.02); } }
@keyframes slight-glitch { 0% { opacity: 1; transform: skewX(0); } 20% { opacity: 0.8; transform: skewX(-5deg); } 22% { opacity: 1; transform: skewX(0); } 100% { opacity: 1; transform: skewX(0); } }
@keyframes melt-drop { 0% { transform: translateY(0); } 50% { transform: translateY(5px); opacity: 0.8; } 100% { transform: translateY(0); } }
@keyframes glitch-anim-1 { 0% { clip: rect(20px, 9999px, 85px, 0); } 20% { clip: rect(66px, 9999px, 32px, 0); } 40% { clip: rect(10px, 9999px, 95px, 0); } 60% { clip: rect(55px, 9999px, 11px, 0); } 80% { clip: rect(82px, 9999px, 49px, 0); } 100% { clip: rect(27px, 9999px, 73px, 0); } }
@keyframes glitch-anim-2 { 0% { clip: rect(89px, 9999px, 12px, 0); } 20% { clip: rect(34px, 9999px, 78px, 0); } 40% { clip: rect(90px, 9999px, 21px, 0); } 60% { clip: rect(15px, 9999px, 63px, 0); } 80% { clip: rect(72px, 9999px, 44px, 0); } 100% { clip: rect(38px, 9999px, 86px, 0); } }

/* ----------------------------------------------------------------------------
   [响应式降维防御 (Responsive Degradation)]
   ---------------------------------------------------------------------------- */
@media (max-width: 1400px) {
  .interface-grid { grid-template-columns: 280px 1fr 300px; padding: 2rem; gap: 1.5rem; }
  .massive-percent .num { font-size: 7rem; }
}
@media (max-width: 1024px) {
  .interface-grid {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto auto auto;
    grid-template-areas: "header" "center" "left" "right" "footer";
    overflow-y: auto; height: 100%;
  }
  .left-pillar-sect, .right-metrics-sect { border: none; padding: 0; border-top: 1px dashed var(--c-muted); padding-top: 2rem; }
  .center-rift-sect { height: 400px; }
  .massive-percent .num { font-size: 5rem; }
}
</style>