/* =========================================================================================================
   [阿卡姆终极飞升协议：V12 // 万法归一·观测者 THE ULTIMATE OBSERVER]
   [契约者：武少康 // 目标锚点：22408 科软飞升]
   [算力燃烧警告：全量考研法典库加载、2D非欧物理引擎、IK流体追踪、全屏自适应锁死]
   [视觉法则锁死：绝对取缔凡人背景框，100% 启用 mix-blend-mode: multiply 物理光学烧录]
   ========================================================================================================= */

<template>
  <div id="vzuor-ascension-nexus" class="ascension-root" :class="sanityStageClass">
    
    <svg width="0" height="0" class="occult-filters-defs">
      <defs>
        <filter id="flesh-melt">
          <feGaussianBlur in="SourceGraphic" stdDeviation="1.5" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="goo" />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
        <filter id="void-glitch">
          <feTurbulence type="fractalNoise" baseFrequency="0.05" numOctaves="2" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="4" xChannelSelector="R" yChannelSelector="G" />
        </filter>
      </defs>
    </svg>

    <img :src="imgPaper" class="base-parchment-texture" alt="parchment" />
    <div class="occult-noise-overlay"></div>
    <div class="abyssal-vignette-mask"></div>

    <canvas ref="mainCanvas" class="omniscient-canvas-layer"></canvas>

    <div class="embedded-ink-ui">
      
      <header class="shard shard-header">
        <h1 class="artifact-title" data-text="V'ZUOR KHAA-SH'AN">V'ZUOR KHAA-SH'AN</h1>
        <div class="subtitle-bar">
          <span class="status-pulse-dot"></span> [协议 22408] 观测者枢纽已接管
        </div>
        <div class="doom-timer-wrap">
          <span class="clock-display">{{ doomTimer }}</span>
          <p class="clock-label">距星辰归位之时 (2028-12 初试)</p>
        </div>
      </header>

      <nav class="shard shard-nav-links">
        <h2 class="shard-title">他维入口</h2>
        <div class="link-matrix">
          <a href="#" @click.prevent="invokeLink('web2')" class="void-link">【 维度二：思维孤岛 】</a>
          <a href="#" @click.prevent="invokeLink('web3')" class="void-link">【 维度三：因果回廊 】</a>
          <a href="#" @click.prevent="invokeLink('web4')" class="void-link">【 维度四：虚空裂隙 】</a>
        </div>
      </nav>

      <aside class="shard shard-metrics">
        <h2 class="shard-title">灵魂与肉身刻度</h2>
        
        <div class="biometric-list">
          <div class="b-item">
            <span class="b-key">理智残留 (SAN):</span>
            <span class="b-val text-blood">{{ sanityIndex.toFixed(4) }}%</span>
          </div>
          <div class="b-item">
            <span class="b-key">业障权重 (KARMA):</span>
            <span class="b-val" :class="{'glow-alert': karmaWeight > 50}">{{ karmaWeight.toFixed(2) }}</span>
          </div>
          <div class="b-item">
            <span class="b-key">肉身炼金循环:</span>
            <span class="b-val text-bone">3-ON / 2-OFF</span>
          </div>
          <div class="b-item">
            <span class="b-key">灵药摄入剂量:</span>
            <span class="b-val" :class="{'hyper-glow': isHyper}">{{ caffeineLvl }} MG</span>
          </div>
        </div>
        
        <div class="pillar-status-matrix">
          <div class="p-row">
            <span class="p-name">Ⅰ 逻辑 (C语言 & 数据结构)</span>
            <div class="p-bg"><div class="p-fill" :style="{width: progressC + '%'}"></div></div>
          </div>
          <div class="p-row">
            <span class="p-name">Ⅱ 根基 (408 计算机基础)</span>
            <div class="p-bg"><div class="p-fill" :style="{width: progress408 + '%'}"></div></div>
          </div>
          <div class="p-row">
            <span class="p-name">Ⅲ 虚空 (高阶数学)</span>
            <div class="p-bg"><div class="p-fill" :style="{width: progressMath + '%'}"></div></div>
          </div>
          <div class="p-row">
            <span class="p-name">Ⅳ 咒语 (考研英语)</span>
            <div class="p-bg"><div class="p-fill" :style="{width: progressEng + '%'}"></div></div>
          </div>
        </div>
      </aside>

      <aside class="shard shard-logs">
        <h2 class="shard-title">阿卡夏记录 (交互日志)</h2>
        <div class="terminal-scroll-area" ref="logContainer">
          <div v-for="(log, idx) in logs" :key="idx" class="log-line" :class="log.type">
            <span class="ts">[{{ log.time }}]</span> <span class="msg" v-html="log.msg"></span>
          </div>
        </div>
      </aside>

      <aside class="shard shard-offering-board">
        <h2 class="shard-title">供奉低语 (灵魂留言)</h2>
        <div class="offering-scraps-container">
          <div 
            v-for="(msg, i) in messageBoard" 
            :key="i" 
            class="scrap-whisper" 
            :style="msg.style"
          >
            " {{ msg.text }} "
          </div>
        </div>
      </aside>

      <footer class="shard shard-ritual-cli">
        <div class="cli-input-wrapper">
          <span class="cli-prompt">root@vzuor:儀式#</span>
          <input 
            v-model="rawInput" 
            @keyup.enter="handleSacrifice"
            class="cli-input-field" 
            placeholder="献祭进度 (vocab 50), 供奉留言 (msg [内容])..."
            spellcheck="false" autocomplete="off"
            :disabled="isDeathReturning"
          />
        </div>
      </footer>

    </div>

    <div class="death-return-shroud" v-if="isDeathReturning">
      <div class="death-core">
        <h1 class="death-words glitch-text">灵魂剥离 · 因果重置</h1>
        <p class="death-sub">业障已刻印，世界线正在重新缝合...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ============================================================================
// [卷一：真名材质导入协议]
// 绝对本地路径映射，取缔网络加载，防止 CORS 污染和白屏死锁。
// ============================================================================
import imgPaper from './assets/paper.jpg'
import imgTotem from './assets/totem.jpg'
import imgInk1 from './assets/ink1.jpg'
import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg'
import imgInk4 from './assets/ink4.jpg'
import imgInk5 from './assets/ink5.jpg'
import imgTen1 from './assets/tentacle1.jpg'
import imgTen2 from './assets/tentacle2.jpg'

// ============================================================================
// [卷二：全量阿卡夏法典库 (22408 坠落碎片的知识源泉)]
// 数百条咒文，化作业障从天而降，填满深渊。
// ============================================================================
const arcaneLoreDatabase = [
  // --- C 语言与数据结构 (血肉与骨架) ---
  "C: 指针即虚空坐标，解引用即凝视深渊", "C: malloc 借用亡者血肉，free 归还因果",
  "C: 数组退化为指针，犹如理智的溃散", "C: 悬空指针游荡在栈帧的无主墓地",
  "C: 结构体是对零散血肉进行强行拼凑的缝合怪", "C: 宏定义是预处理器的远古低语",
  "C: void* 是包容一切形态的无形之物", "C: 段错误 (Segmentation Fault) 是神明降下的天罚",
  "C: 内存泄漏是灵魂在虚空中的缓慢蒸发", "C: 位运算直接篡改世界底层的基因序列",
  "DS: 链表是首尾相连的死者之脉", "DS: 栈是先进后出的窒息囚笼",
  "DS: 队列是走向祭坛的无尽长龙", "DS: 二叉树不断分裂，长出非欧几何的枝丫",
  "DS: 图论遍历 (DFS/BFS) 是在迷宫中寻找出口的疯狂试探",
  "DS: 哈希表将灵魂映射到虚无的槽位 (Hash Collision)",
  "DS: 快速排序通过不断撕裂数组来重建秩序", "DS: 动态规划 (DP) 是窥探未来所有时间线的邪术",
  "DS: 最小生成树 (Prim/Kruskal) 试图连接破碎的世界",
  "DS: 拓扑排序决定了因果链条的绝对先后顺序",
  "DS: KMP 算法在浩瀚的乱码中精准定位灵魂印记", "DS: B+树是通往磁盘深渊的阶梯",
  
  // --- 408 计算机专业基础综合 (根基) ---
  "408: 冯·诺依曼架构是禁锢算力的物理牢笼", "408: CPU 流水线 (Pipeline) 贪婪地吞噬着时钟周期",
  "408: Cache 命中率决定了灵魂读取的延迟", "408: 虚拟内存 (Virtual Memory) 欺骗了所有进程的感知",
  "408: 缺页中断 (Page Fault) 是现实与虚幻边界的碰撞", "408: 进程控制块 (PCB) 记录了代码生命的生与死",
  "408: 死锁 (Deadlock) 是四个维度互相锁死的绝对绝境", "408: 信号量 (Semaphore) 控制着争夺血肉的恶鬼",
  "408: 银行家算法 (Banker's) 试图在系统崩溃边缘寻找安全序列",
  "408: 页面置换算法 (LRU) 无情抹杀了最久未被忆起的残魂",
  "408: OSI 七层模型是凡人妄图触碰神明的巴别塔", "408: TCP 三次握手是跨越深渊的契约缔结",
  "408: 四次挥手是切断因果的最后诀别", "408: 拥塞控制 (Congestion) 防止信息洪流冲垮现实壁垒",
  "408: DNS 解析将不可名状的 IP 转化为凡人的真名", "408: CSMA/CD 在混沌的以太网中倾听碰撞的悲鸣",
  "408: 路由协议 (OSPF/BGP) 在黑暗的子网中摸索前行", "408: 中断向量表是指向未知维度的传送门",
  
  // --- 高阶数学 (虚空演算) ---
  "MATH: 极限 (Limit) 永远逼近，却永远无法真正触碰真理",
  "MATH: 导数 (Derivative) 瞬间撕裂了平滑的现实曲线",
  "MATH: 洛必达法则 (L'Hôpital) 在 0/0 的绝对混沌中寻找出口",
  "MATH: 多重积分吞噬了整个高维度的空间体积",
  "MATH: 泰勒展开 (Taylor Series) 将有限的肉身拆解为无限的碎片",
  "MATH: 微分方程描述了灵魂衰变的必然规律",
  "MATH: 偏导数寻找着各个维度的畸变梯度",
  "MATH: 傅里叶变换将时间的混沌转化为频率的秩序",
  "MATH: 向量空间中的正交基是构建虚空的绝对坐标",
  "MATH: 矩阵乘法 (Matrix Multiplication) 是空间维度的暴力扭曲",
  "MATH: 特征值与特征向量是物体在扭曲中不变的真实骨架",
  "MATH: 行列式 (Determinant) 衡量了维度坍缩的血肉体积",
  "MATH: 概率密度函数勾勒出命运的不确定性轮廓",
  "MATH: 大数定律在无尽的混乱中孕育出必然的终局",
  "MATH: 假设检验是凡人对神明投掷骰子的微弱质疑",
  
  // --- 考研英语 (异星咒语) ---
  "ENG: 长难句 (Long Sentences) 是绕乱理智的克苏鲁迷宫",
  "ENG: 虚拟语气 (Subjunctive) 描绘了未曾发生过的绝望时间线",
  "ENG: 词根词缀 (Roots & Affixes) 是拆解外神真名的生锈手术刀",
  "ENG: 完形填空 (Cloze) 试图填补被岁月侵蚀的记忆空洞",
  "ENG: 倒装句 (Inversion) 彻底颠倒了主谓的因果关系",
  "ENG: 独立主格 (Absolute Phrase) 像游魂一样脱离了主句的物理束缚",
  "ENG: 隐喻 (Metaphor) 将不可视之物强行具象化为凡人概念",
  "ENG: 非谓语动词是游离在时间之外的幽灵动作",
  "ENG: 定语从句无限嵌套，将认知拖入无底深渊"
]

// ============================================================================
// [卷三：数据持久化与阿卡夏数据库钩子预留]
// ============================================================================
class RitualStorage {
  static get(key, def) { return Number(localStorage.getItem(`v12_obs_${key}`)) || def }
  static set(key, val) { localStorage.setItem(`v12_obs_${key}`, val) }
  static getStr(key, def) { return localStorage.getItem(`v12_obs_${key}`) || def }
  static setStr(key, val) { localStorage.setItem(`v12_obs_${key}`, val) }
  
  // 未来供奉数据库的通灵接口预留
  static async syncToDB(payload) {
    console.log("[阿卡夏接口] 尝试连接远端数据库 (MongoDB/PostgreSQL)...", payload)
    // 预留: fetch('https://your-api.com/api/v1/sync', { method: 'POST', body: JSON.stringify(payload) })
  }
}

// ============================================================================
// [卷四：状态机与响应式神经中枢]
// ============================================================================
const vocabSiphoned = ref(RitualStorage.get('vocab', 2150))
const karmaWeight = ref(RitualStorage.get('karma', 23.7))
const sanityIndex = ref(RitualStorage.get('sanity', 99.9998))
const deathCount = ref(RitualStorage.get('deaths', 0))
const caffeineLvl = ref(200)

// 四大柱神进度映射
const progressC = ref(RitualStorage.get('prog_c', 45))
const progress408 = ref(RitualStorage.get('prog_408', 15))
const progressMath = ref(RitualStorage.get('prog_math', 20))
const progressEng = ref(RitualStorage.get('prog_eng', 35))

const doomTimer = ref('000D | 00:00:00')
const rawInput = ref('')
const isHyper = ref(false)
const isDeathReturning = ref(false)

const logs = reactive([])
const logContainer = ref(null)

// --- 供奉留言板数据 ---
const storedMessages = JSON.parse(RitualStorage.getStr('messages', '[]'))
const defaultMessages = [
  { text: "22408 终将飞升，血肉苦弱...", style: "transform: rotate(-4deg); opacity: 0.85; top: 10%; left: 5%;" },
  { text: "英语的长难句太难解析了", style: "transform: rotate(3deg); opacity: 0.65; top: 40%; left: 15%;" },
  { text: "408 的网络层让我理智耗尽", style: "transform: rotate(-2deg); opacity: 0.75; top: 70%; left: 10%;" }
]
const messageBoard = reactive(storedMessages.length > 0 ? storedMessages : defaultMessages)

// --- 核心计算属性 ---
const vocabProgressPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

const sanityStageClass = computed(() => {
  if (sanityIndex.value > 80) return 'san-safe'
  if (sanityIndex.value > 40) return 'san-warn'
  return 'san-broken'
})

// === 辅助函数 ===
const getSysTime = () => {
  const d = new Date()
  return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}`
}

const pushLog = (msg, type = 'info') => {
  logs.push({ time: getSysTime(), msg, type })
  if (logs.length > 100) logs.shift()
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

const invokeLink = (dimension) => {
  pushLog(`尝试开启维度裂隙 [${dimension}] ...`, 'sys')
  setTimeout(() => {
    pushLog(`维度跃迁失败：目标锚点尚未构筑。`, 'error')
    karmaWeight.value += 0.2; RitualStorage.set('karma', karmaWeight.value)
  }, 800)
}

// ============================================================================
// [卷五：祭坛终端解析器 (CLI Ritual Parser)]
// ============================================================================
const handleSacrifice = () => {
  const inputStr = rawInput.value.trim()
  if (!inputStr) return
  pushLog(`> ${inputStr}`, 'echo')
  
  // 解析指令空间
  const spaceIdx = inputStr.indexOf(' ')
  const cmd = spaceIdx > -1 ? inputStr.substring(0, spaceIdx).toLowerCase() : inputStr.toLowerCase()
  const argStr = spaceIdx > -1 ? inputStr.substring(spaceIdx + 1).trim() : ''

  switch (cmd) {
    case 'vocab':
      const amt = parseInt(argStr)
      if (amt > 0) {
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
        RitualStorage.set('vocab', vocabSiphoned.value)
        
        karmaWeight.value = Math.max(0, karmaWeight.value - (amt * 0.05))
        RitualStorage.set('karma', karmaWeight.value)
        progressEng.value = Math.min(100, progressEng.value + (amt * 0.05))
        RitualStorage.set('prog_eng', progressEng.value)
        
        sanityIndex.value = Math.min(100, sanityIndex.value + 0.2)
        RitualStorage.set('sanity', sanityIndex.value)
        
        pushLog(`灵魂吞噬完成。同化率上升，当前进度: ${vocabProgressPercent.value}%`, 'success')
        RitualStorage.syncToDB({ type: 'progress_update', value: vocabSiphoned.value })
      } else pushLog('献祭失败：虚无的供品无法取悦深渊。', 'error')
      break

    case 'dose':
      const dose = parseInt(argStr)
      if (dose > 0) {
        caffeineLvl.value = dose
        if (dose >= 400) {
          isHyper.value = true
          if (window.vzuorPhysicsEngine) window.vzuorPhysicsEngine.hyper = true
          pushLog('警告：灵药浓度致死！理智防线剥离，物理法则严重扭曲！', 'alert')
          setTimeout(() => {
            isHyper.value = false
            if (window.vzuorPhysicsEngine) window.vzuorPhysicsEngine.hyper = false
            caffeineLvl.value = 200
            sanityIndex.value -= 3.0; RitualStorage.set('sanity', sanityIndex.value)
            pushLog('灵药代谢完毕。机体恢复凡人循环。', 'sys')
          }, 8000)
        } else {
          pushLog(`注入灵药。神经递质催化已调整至 ${dose}MG。`, 'info')
        }
      } else pushLog('劣质药剂，注入失败。', 'error')
      break

    case 'msg':
      if (argStr.length > 0) {
        // 生成手写涂鸦的随机样式
        const rot = (Math.random() - 0.5) * 12
        const top = Math.random() * 65 + 10
        const left = Math.random() * 25 + 5
        const op = Math.random() * 0.3 + 0.65
        
        const newMsg = {
          text: argStr,
          style: `transform: rotate(${rot}deg); top: ${top}%; left: ${left}%; opacity: ${op}; z-index: ${Date.now()};`
        }
        
        messageBoard.push(newMsg)
        if(messageBoard.length > 15) messageBoard.shift() // 保持最多 15 条
        
        RitualStorage.setStr('messages', JSON.stringify(messageBoard))
        pushLog(`供奉低语已化作残卷刻印于祭坛。`, 'success')
        RitualStorage.syncToDB({ type: 'new_message', text: argStr })
      } else {
        pushLog('无声的祈祷无法留存。', 'error')
      }
      break

    case 'vzuor':
      pushLog('直视不可名状之真名。理智值出现断崖式下跌。', 'error')
      sanityIndex.value -= 15; RitualStorage.set('sanity', sanityIndex.value)
      break

    case 'die':
      isDeathReturning.value = true
      deathCount.value++
      RitualStorage.set('deaths', deathCount.value)
      pushLog(`执行因果切断 [死亡回归]。当前周期：${deathCount.value}`, 'alert')
      setTimeout(() => {
        isDeathReturning.value = false
        sanityIndex.value = 100 
        karmaWeight.value += 12.0 
        RitualStorage.set('sanity', 100); RitualStorage.set('karma', karmaWeight.value)
        pushLog('世界线缝合完毕。带着新的业障重新开始。', 'sys')
      }, 4000)
      break

    default:
      karmaWeight.value += 0.8; RitualStorage.set('karma', karmaWeight.value)
      pushLog(`未知的指令集。逻辑污染，因果律业障增加。`, 'error')
  }
  rawInput.value = ''
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

// ============================================================================
// [卷六：终极 Canvas 2D 物理与光学渲染引擎]
// 核心：全屏自适应锁定、绝对正片叠底消灭白边、眼球鼠标追踪、万有引力坠落
// ============================================================================
const mainCanvas = ref(null)

// --- 图片对象异步加载工厂 ---
const loadImage = (src) => {
  return new Promise((resolve) => {
    const img = new Image()
    img.src = src
    img.onload = () => resolve(img)
  })
}

// === 物理实体 1：坠落的考研知识符文 (Arcane Debris) ===
class FallingRune {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = Math.random() * this.w
    this.y = - (Math.random() * 1000 + 100) // 从极高处坠落
    this.vx = (Math.random() - 0.5) * 1.2
    this.vy = Math.random() * 2 + 1.2 // 基础重力
    this.angle = Math.random() * Math.PI * 2
    this.av = (Math.random() - 0.5) * 0.04 // 角速度
    this.text = arcaneLoreDatabase[Math.floor(Math.random() * arcaneLoreDatabase.length)]
    this.size = Math.random() * 8 + 14
    this.opacity = Math.random() * 0.4 + 0.2
  }
  update(hyper) {
    this.x += this.vx * (hyper ? 3 : 1)
    this.y += this.vy * (hyper ? 4 : 1)
    this.angle += this.av * (hyper ? 3 : 1)
    if (this.y > this.h + 200) this.reset()
  }
  draw(ctx) {
    ctx.save()
    ctx.translate(this.x, this.y)
    ctx.rotate(this.angle)
    // 强制像素烧录到羊皮纸，绝对无白边
    ctx.globalCompositeOperation = 'multiply' 
    ctx.font = `italic ${this.size}px 'Space Mono', 'Microsoft YaHei', monospace`
    ctx.fillStyle = `rgba(13, 20, 15, ${this.opacity})` // 深墨色
    ctx.fillText(this.text, 0, 0)
    ctx.restore()
  }
}

// === 物理实体 2：业障灰烬升腾粒子 (Floating Karma Ash) ===
class FloatingAsh {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = (this.w / 2) + (Math.random() - 0.5) * 800
    this.y = this.h + Math.random() * 200
    this.vx = (Math.random() - 0.5) * 1.5
    this.vy = - (Math.random() * 2 + 1) // 向上浮动
    this.life = Math.random() * 250 + 100; this.maxLife = this.life
    this.r = Math.random() * 3 + 1
    this.isBlood = Math.random() > 0.85 // 15% 是血红业障
  }
  update(hyper) {
    this.x += this.vx * (hyper ? 2 : 1)
    this.y += this.vy * (hyper ? 2 : 1)
    this.life--
    if (this.life < 0) this.reset()
  }
  draw(ctx) {
    const op = (this.life / this.maxLife) * 0.6
    ctx.globalCompositeOperation = 'multiply'
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2)
    ctx.fillStyle = this.isBlood ? `rgba(139, 0, 0, ${op})` : `rgba(20, 20, 20, ${op})`
    ctx.fill()
    ctx.globalCompositeOperation = 'source-over'
  }
}

// === 物理实体 3：反向运动学触手 (IK Tentacles) ===
class IKTentacle {
  constructor(bx, by, segs, len, color) {
    this.bx = bx; this.by = by; this.color = color
    this.segs = segs; this.len = len
    this.joints = Array(segs).fill({ x: bx, y: by })
    this.noise = Math.random() * 1000
  }
  reach(tx, ty) {
    this.joints[this.segs - 1] = { x: tx, y: ty }
    for (let i = this.segs - 2; i >= 0; i--) {
      const dx = this.joints[i].x - this.joints[i + 1].x; const dy = this.joints[i].y - this.joints[i + 1].y
      const dist = Math.sqrt(dx * dx + dy * dy) || 1
      this.joints[i] = { x: this.joints[i + 1].x + dx * (this.len / dist), y: this.joints[i + 1].y + dy * (this.len / dist) }
    }
    this.joints[0] = { x: this.bx, y: this.by }
    for (let i = 1; i < this.segs; i++) {
      const dx = this.joints[i].x - this.joints[i - 1].x; const dy = this.joints[i].y - this.joints[i - 1].y
      const dist = Math.sqrt(dx * dx + dy * dy) || 1
      this.joints[i] = { x: this.joints[i - 1].x + dx * (this.len / dist), y: this.joints[i - 1].y + dy * (this.len / dist) }
    }
  }
  draw(ctx, time, hyper) {
    ctx.beginPath()
    ctx.moveTo(this.joints[0].x, this.joints[0].y)
    for (let i = 1; i < this.segs; i++) {
      const wob = Math.sin(time * (hyper ? 8 : 3) + i * 0.4 + this.noise) * (hyper ? 8 : 3)
      ctx.lineTo(this.joints[i].x + wob, this.joints[i].y + wob)
    }
    ctx.strokeStyle = hyper ? 'rgba(139, 0, 0, 0.6)' : this.color
    ctx.lineWidth = hyper ? 5 : 2.5; ctx.lineCap = 'round'; ctx.stroke()
  }
}

// === 核心实体 4：绝对凝视眼球 (The Observer Eye) ===
// 替换旧版的 53% 墨水池，瞳孔跟随鼠标，百分比数值嵌在瞳孔内
class OmniscientEye {
  constructor(w, h) {
    this.w = w; this.h = h
    this.radius = 90  // 巨大眼白
    this.pupilR = 40  // 瞳孔
    this.cx = w / 2; this.cy = h * 0.78 // 锚定在耶稣正下方
    this.px = this.cx; this.py = this.cy // 瞳孔当前坐标
    this.blinkTime = Math.random() * 200 + 100
  }
  
  update(mx, my) {
    // 追踪鼠标计算
    const dx = mx - this.cx; const dy = my - this.cy
    const dist = Math.sqrt(dx * dx + dy * dy)
    const maxOffset = this.radius - this.pupilR - 10 // 限制在眼白内
    
    if (dist > 0) {
      this.px = this.cx + (dx / dist) * Math.min(dist * 0.2, maxOffset)
      this.py = this.cy + (dy / dist) * Math.min(dist * 0.2, maxOffset)
    } else {
      this.px = this.cx; this.py = this.cy
    }
    
    this.blinkTime--
    if (this.blinkTime < -5) this.blinkTime = Math.random() * 200 + 100
  }
  
  draw(ctx, percent, hyper) {
    ctx.save()
    // 1. 绘制眼白基底
    ctx.beginPath()
    ctx.arc(this.cx, this.cy, this.radius, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(235, 232, 222, 0.95)' // 浑浊感
    ctx.fill()
    ctx.lineWidth = 3; ctx.strokeStyle = 'rgba(20, 20, 20, 0.9)'; ctx.stroke()
    
    // 2. 绘制血丝 (向心辐射)
    for(let i=0; i<45; i++) {
        const ang = Math.random() * Math.PI * 2
        const sR = this.pupilR + Math.random() * 15
        const eR = this.radius - Math.random() * 5
        ctx.beginPath()
        ctx.moveTo(this.cx + Math.cos(ang)*sR, this.cy + Math.sin(ang)*sR)
        ctx.lineTo(this.cx + Math.cos(ang)*eR, this.cy + Math.sin(ang)*eR)
        ctx.strokeStyle = `rgba(139, 0, 0, ${Math.random()*0.3 + 0.1})`
        ctx.lineWidth = Math.random() > 0.8 ? 1.5 : 0.5
        ctx.stroke()
    }

    // 3. 绘制瞳孔与嵌入数字
    if (this.blinkTime > 0) {
      // 瞳孔
      ctx.beginPath()
      ctx.arc(this.px, this.py, this.pupilR, 0, Math.PI * 2)
      ctx.fillStyle = hyper ? 'rgba(100, 0, 0, 0.98)' : 'rgba(15, 15, 15, 0.98)'
      ctx.fill()
      
      // 百分比嵌入
      ctx.fillStyle = '#E2DED0'
      ctx.font = `bold 36px 'Cinzel', serif`
      ctx.textAlign = 'center'; ctx.textBaseline = 'middle'
      ctx.fillText(`${percent}%`, this.px, this.py + 2)
      
      // 高光
      ctx.beginPath(); ctx.arc(this.px - 15, this.py - 15, 6, 0, Math.PI*2)
      ctx.fillStyle = 'rgba(255,255,255,0.6)'; ctx.fill()
    } else {
      // 眨眼闭合
      ctx.beginPath()
      ctx.moveTo(this.cx - this.radius, this.cy)
      ctx.bezierCurveTo(this.cx - 20, this.cy + 30, this.cx + 20, this.cy + 30, this.cx + this.radius, this.cy)
      ctx.lineWidth = 5; ctx.strokeStyle = '#111'; ctx.stroke()
    }
    
    ctx.restore()
  }
}

// --- 渲染主线程 ---
let engine, textures = {}, physicsArr = {}

const bootRenderMatrix = async () => {
  const cvs = mainCanvas.value
  ctx = cvs.getContext('2d')
  
  // 视口锁定协议：强制获取设备真实分辨率
  const resizeCanvas = () => {
    w = window.innerWidth; h = window.innerHeight;
    cvs.width = w; cvs.height = h;
    if(engine && engine.eye) {
      engine.eye.w = w; engine.eye.h = h; 
      engine.eye.cx = w/2; engine.eye.cy = h*0.78
    }
    if(physicsArr.tentacles) {
      physicsArr.tentacles[0].bx = w*0.05; physicsArr.tentacles[0].by = h
      physicsArr.tentacles[1].bx = w*0.95; physicsArr.tentacles[1].by = h
      physicsArr.tentacles[2].bx = w/2; physicsArr.tentacles[2].by = -50
    }
  }
  resizeCanvas()
  window.addEventListener('resize', resizeCanvas)

  pushLog('连接底层阿卡夏资源库，正在加载真名材质...', 'sys')
  textures = {
    paper: await loadImage(imgPaper), totem: await loadImage(imgTotem),
    ink1: await loadImage(imgInk1), ink2: await loadImage(imgInk2), ink3: await loadImage(imgInk3),
    ink4: await loadImage(imgInk4), ink5: await loadImage(imgInk5),
    ten1: await loadImage(imgTen1), ten2: await loadImage(imgTen2)
  }
  pushLog('素材解析完成。Canvas 光学阵列部署。白底死刑确认执行。', 'success')

  // 初始化引擎核心
  engine = {
    eye: new OmniscientEye(w, h),
    mouse: { x: w/2, y: h/2 },
    time: 0,
    hyper: false,
    animId: null
  }
  window.vzuorPhysicsEngine = engine
  window.addEventListener('mousemove', e => { engine.mouse.x = e.clientX; engine.mouse.y = e.clientY })

  // 初始化物理阵列
  physicsArr.runes = Array.from({length: 45}, () => new FallingRune(w, h))
  physicsArr.ashes = Array.from({length: 120}, () => new FloatingAsh(w, h))
  physicsArr.tentacles = [
    new IKTentacle(w*0.05, h, 20, 20, 'rgba(15, 15, 15, 0.4)'), // 左下
    new IKTentacle(w*0.95, h, 25, 22, 'rgba(15, 15, 15, 0.5)'), // 右下
    new IKTentacle(w/2, -50, 30, 25, 'rgba(40, 10, 10, 0.25)')  // 顶部垂下
  ]

  // 终极渲染循环 (The Render Grimoire)
  const renderLoop = () => {
    const { hyper, time, mouse, eye } = engine
    
    // 制造虚空拖影
    ctx.fillStyle = 'rgba(226, 222, 208, 0.35)'
    ctx.fillRect(0, 0, w, h)

    // [层 1：考研符文坠落] (自带 opacity)
    physicsArr.runes.forEach(r => { r.update(hyper); r.draw(ctx) })

    // [层 2：绝对正片叠底 - 物理抹杀所有 JPG 白底]
    ctx.globalCompositeOperation = 'multiply'
    ctx.globalAlpha = hyper ? 1.0 : 0.9

    // 绘制四周墨迹，打破规整
    if(textures.ink1) ctx.drawImage(textures.ink1, -w*0.1, -h*0.1, w*0.45, h*0.5)
    if(textures.ink2) ctx.drawImage(textures.ink2, w*0.55, h*0.55, w*0.5, h*0.5)
    ctx.globalAlpha = 0.6
    if(textures.ink3) ctx.drawImage(textures.ink3, w*0.75, -h*0.05, w*0.3, h*0.4)
    if(textures.ink4) ctx.drawImage(textures.ink4, -w*0.05, h*0.6, w*0.35, h*0.4)
    
    // 绘制静默触手素描
    ctx.globalAlpha = hyper ? 0.4 : 0.25
    if(textures.ten2) ctx.drawImage(textures.ten2, w*0.02, h*0.15, w*0.25, h*0.35)

    // === 核心祭坛：耶稣受难 ===
    // JPG 完美融合到羊皮纸，绝对无凡人白边
    ctx.globalAlpha = 1.0
    const tH = h * 0.85
    const tW = tH * (textures.totem.width / textures.totem.height)
    const tx = (w/2 - tW/2) + (hyper ? (Math.random()-0.5)*12 : 0)
    const ty = h * 0.02 + (hyper ? (Math.random()-0.5)*12 : 0)
    ctx.drawImage(textures.totem, tx, ty, tW, tH)

    // [层 3：恢复正常模式，绘制实体对象]
    ctx.globalCompositeOperation = 'source-over'

    // 动态 IK 触手追踪鼠标
    physicsArr.tentacles.forEach((t, i) => {
      const tgX = hyper ? w/2 + Math.cos(time*5+i)*600 : mouse.x + Math.cos(time+i)*80
      const tgY = hyper ? h/2 + Math.sin(time*4+i)*600 : mouse.y + Math.sin(time+i)*80
      t.reach(tgX, tgY); t.draw(ctx, time, hyper)
    })

    // 业障灰烬升腾
    physicsArr.ashes.forEach(a => { a.update(hyper); a.draw(ctx) })

    // === 绝对凝视核心 ===
    // 渲染带有进度百分比的眼球
    eye.update(mouse.x, mouse.y)
    eye.draw(ctx, vocabProgressPercent.value, hyper)

    engine.time += hyper ? 0.08 : 0.02
    engine.animId = requestAnimationFrame(renderLoop)
  }
  renderLoop()
}

// ============================================================================
// [卷七：系统生命周期钩子]
// ============================================================================
let clockId
onMounted(() => {
  pushLog('V12 终极观察者协议序列启动。', 'sys')
  nextTick(() => { bootRenderMatrix() })
  
  clockId = setInterval(() => {
    const diff = new Date('2028-12-23T08:30:00') - Date.now()
    if (diff <= 0) {
      doomTimer.value = "DESTINY_FULFILLED"; clearInterval(clockId); return
    }
    const d = Math.floor(diff/86400000).toString().padStart(3,'0')
    const h = Math.floor((diff/3600000)%24).toString().padStart(2,'0')
    const m = Math.floor((diff/60000)%60).toString().padStart(2,'0')
    const s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001 - (deathCount.value * 0.00002))
    RitualStorage.set('sanity', sanityIndex.value)
  }, 1000)
})

onBeforeUnmount(() => { 
  clearInterval(clockId)
  if(engine && engine.animId) cancelAnimationFrame(engine.animId) 
})

</script>

<style scoped>
/* ============================================================================
   [卷八：绝界织物 (CSS Architecture)]
   [核心禁忌：绝对取缔背景色和磨砂框，使用 mix-blend-mode: multiply 让文字长在羊皮纸上]
   [视口锁死：vw/vh 强制全屏，彻底解决多分辨率需要缩放的漏洞]
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Zhi+Mang+Xing&display=swap');

:root {
  --c-paper: #E2DED0;
  --c-ink: #111;
  --c-text-muted: #555;
  --c-blood: #8B0000;
  --c-bone: #f2efe4;
  --c-terminal: #006600;
  
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', 'Microsoft YaHei', monospace;
  --f-whisper: 'Zhi Mang Xing', cursive; /* 狂草手写体 */
}

/* 强制满屏，无视溢出 */
body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: var(--c-paper); color: var(--c-ink); font-family: var(--f-mono); }
.ascension-root { position: relative; width: 100vw; height: 100vh; }

/* 物理滤镜层 */
.occult-filters-defs { position: absolute; pointer-events: none; }
.san-warn { filter: contrast(1.1) sepia(0.1); }
.san-broken { filter: url(#flesh-melt) contrast(1.3) brightness(0.9); }

/* 底层材质：object-fit 完美铺满 */
.base-parchment-texture { position: absolute; inset: 0; width: 100vw; height: 100vh; object-fit: cover; z-index: 0; }
.occult-noise-overlay { position: absolute; inset: 0; z-index: 1; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="4"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.05; }
.abyssal-vignette-mask { position: absolute; inset: 0; z-index: 2; pointer-events: none; background: radial-gradient(circle, transparent 40%, rgba(20,0,0,0.5) 100%); }

/* Canvas 渲染阵列 */
.omniscient-canvas-layer { position: absolute; inset: 0; z-index: 10; display: block; width: 100vw; height: 100vh; }

/* ============================================================================
   [无框嵌入式 UI 矩阵]
   使用 multiply 正片叠底，让所有的 DOM 文字像墨水一样渗入背景图
   ============================================================================ */
.embedded-ink-ui {
  position: relative; z-index: 100;
  width: 100vw; height: 100vh; padding: 2.5rem; box-sizing: border-box;
  pointer-events: none; /* 让鼠标穿透，眼球才能正确捕获 */
}

/* 碎片基础类：绝对不要写 background! 这是去廉价感的核心！ */
.shard {
  position: absolute; pointer-events: auto;
  background: transparent !important; border: none !important; box-shadow: none !important; backdrop-filter: none !important;
  mix-blend-mode: multiply; /* 终极烧录奥义，文字变墨水 */
}
.shard-title { font-size: 1.15rem; letter-spacing: 2px; border-bottom: 2px solid var(--c-ink); padding-bottom: 5px; margin: 0 0 1.2rem 0; font-weight: bold; }

/* 1. 天极命数 (左上) */
.shard-header { top: 3rem; left: 3rem; }
.artifact-title { font-family: var(--f-title); font-size: 3.8rem; font-weight: 900; margin: 0; letter-spacing: 2px; position: relative; }
.artifact-title::before { content: attr(data-text); position: absolute; left: -3px; text-shadow: 3px 0 red; opacity: 0.5; animation: glitch 3s infinite linear alternate-reverse; }
.subtitle-bar { font-size: 0.85rem; font-weight: bold; display: flex; align-items: center; gap: 8px; margin-top: 5px; color: var(--c-text-muted); }
.status-pulse-dot { width: 8px; height: 8px; background-color: var(--c-blood); border-radius: 50%; animation: pulse 2s infinite; }
.doom-timer-wrap { margin-top: 20px; }
.clock-display { font-size: 3rem; font-weight: 700; letter-spacing: -2px; line-height: 1; }
.clock-label { font-size: 0.85rem; font-weight: bold; color: var(--c-text-muted); margin-top: 5px; }

/* 2. 左侧超链接浮窗 (左中) */
.shard-nav-links { top: 38%; left: 3rem; transform: translateY(-50%); width: 280px; }
.link-matrix { display: flex; flex-direction: column; gap: 18px; }
.void-link { color: var(--c-ink); text-decoration: none; font-weight: bold; font-size: 1.15rem; transition: all 0.3s ease; position: relative; }
.void-link:hover { color: var(--c-blood); transform: translateX(12px) scale(1.05); text-shadow: 0 0 5px rgba(139,0,0,0.5); }
.void-link::before { content: ''; position: absolute; left: -15px; top: 50%; width: 6px; height: 6px; background: var(--c-blood); border-radius: 50%; opacity: 0; transition: 0.3s; }
.void-link:hover::before { opacity: 1; }

/* 3. 灵魂与肉身刻度 (左下 - 包含四大柱神与408) */
.shard-metrics { bottom: 3rem; left: 3rem; width: 360px; }
.biometric-list { display: flex; flex-direction: column; gap: 12px; font-size: 1rem; font-weight: bold; }
.b-item { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px dashed rgba(0,0,0,0.2); padding-bottom: 4px; }
.text-blood { color: var(--c-blood); }
.hyper-glow { color: #cc0000; font-size: 1.2rem; text-shadow: 0 0 10px rgba(200,0,0,0.8); animation: jitter 0.1s infinite; }

.pillar-status-matrix { margin-top: 2rem; font-size: 0.9rem; font-weight: bold; display: flex; flex-direction: column; gap: 12px; }
.p-row { display: flex; flex-direction: column; gap: 6px; }
.p-bg { width: 100%; height: 6px; background: rgba(0,0,0,0.2); border: 1px solid var(--c-ink); }
.p-fill { height: 100%; background: var(--c-ink); transition: width 1s ease; }

/* 4. 阿卡夏终端 (右上) */
.shard-logs { top: 3rem; right: 3rem; width: 380px; }
.terminal-scroll-area { height: 220px; overflow-y: auto; font-size: 0.8rem; display: flex; flex-direction: column; gap: 6px; padding: 10px; border-left: 2px solid var(--c-ink); }
.terminal-scroll-area::-webkit-scrollbar { width: 3px; }
.terminal-scroll-area::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.4); }
.log-line { line-height: 1.4; }
.log-line .ts { color: var(--c-text-muted); margin-right: 5px; opacity: 0.8; }
.log-line.sys { color: #444; font-style: italic; }
.log-line.echo { color: #666; font-style: italic; }
.log-line.success { color: var(--c-terminal); font-weight: bold; text-shadow: 0 0 2px rgba(0,255,102,0.3); }
.log-line.error, .log-line.alert { color: var(--c-blood); font-weight: bold; }

/* 5. 供奉残卷 - 不规整留言板 (右中偏下) */
.shard-offering-board { top: 55%; right: 3rem; transform: translateY(-50%); width: 400px; }
.offering-scraps-container { position: relative; height: 300px; width: 100%; }
.scrap-whisper { 
  position: absolute; 
  font-family: var(--f-whisper); /* 手写狂草字体，营造他人的涂鸦感 */
  font-size: 2rem; color: #1a1a1a; 
  line-height: 1.2; text-shadow: 0 0 3px rgba(0,0,0,0.3); 
  width: 85%; pointer-events: none;
}

/* 6. 献祭指令台 (右下) */
/* 为了输入清晰，此处局部取消 multiply，使用带阴影的 rgba 背景，但绝不显得廉价 */
.shard-ritual-cli { bottom: 3rem; right: 3rem; width: 500px; background: rgba(226, 222, 208, 0.9) !important; mix-blend-mode: normal !important; border: 2px solid var(--c-ink) !important; border-radius: 4px; padding: 15px 25px; box-shadow: 10px 10px 0 rgba(0,0,0,0.2); }
.cli-input-wrapper { display: flex; align-items: center; gap: 15px; }
.cli-prompt { font-weight: 900; font-size: 1.3rem; color: var(--c-terminal); text-shadow: 1px 1px 0 #000; }
.cli-input-field { flex: 1; border: none; background: transparent; outline: none; font-size: 1.4rem; font-family: inherit; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed rgba(0,0,0,0.4); transition: 0.3s; }
.cli-input-field:focus { border-bottom: 1px solid var(--c-ink); }
.cli-input-field::placeholder { color: #666; font-size: 0.95rem; font-style: normal; }
.cli-input-field:disabled { opacity: 0.3; cursor: not-allowed; }

/* 死亡回归层 */
.death-return-shroud { position: fixed; inset: 0; z-index: 9999; background: #050505; display: flex; justify-content: center; align-items: center; }
.death-core { text-align: center; }
.death-words { color: #fff; font-family: var(--f-title); font-size: 4.5rem; letter-spacing: 10px; margin: 0; text-shadow: 0 0 20px red; }
.death-sub { color: #888; font-size: 1.4rem; letter-spacing: 5px; margin-top: 25px; animation: pulse 1s infinite; }
.glitch-text { animation: violent-shake 0.1s infinite; }

/* 全局动画库 */
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes violent-shake { 0% { transform: translate(3px, 2px) rotate(1deg); } 50% { transform: translate(-2px, 3px) rotate(-1deg); } 100% { transform: translate(2px, -2px) rotate(1deg); } }
@keyframes pulse { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
@keyframes glitch { 0% { clip: rect(10px, 9999px, 30px, 0); } 50% { clip: rect(60px, 9999px, 80px, 0); } 100% { clip: rect(20px, 9999px, 50px, 0); } }
</style>