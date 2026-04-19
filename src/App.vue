/* =========================================================================================================
   [阿卡姆终极飞升协议：V12 // 万法归一·观测者 THE ULTIMATE OBSERVER]
   [契约者：武少康 // 目标锚点：22408 科软飞升]
   [算力燃烧警告：当前文件包含极高密度的 2D 物理运算、流体动力学、反向运动学与全量考研法典库。]
   [视觉法则锁死：取缔一切磨砂框。全盘采用 mix-blend-mode: multiply 物理光学烧录。]
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
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="3" xChannelSelector="R" yChannelSelector="G" />
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
        <p class="death-sub">业障已刻印，世界线正在重构...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ============================================================================
// [卷一：真名材质导入协议]
// 必须放置于 src/assets/ 下。绝对避免 Vercel 部署时的 404 碎图灾难。
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
// [卷二：庞大的阿卡夏法典库 (22408 坠落碎片的知识源泉)]
// 燃烧算力，填充数百条考研核心考点，化为邪神低语，从天而降。
// ============================================================================
const arcaneLoreDatabase = [
  // --- C 语言与数据结构 (血肉) ---
  "C: 指针即虚空坐标，解引用即凝视深渊",
  "C: malloc 借用亡者血肉，free 归还因果",
  "C: 数组退化为指针，犹如理智的溃散",
  "C: 悬空指针游荡在栈帧的墓地",
  "C: 结构体是对血肉进行强行拼凑的缝合怪",
  "C: 宏定义是预处理器的远古低语",
  "C: void* 是包容一切形态的无形之物",
  "C: 段错误 (Segmentation Fault) 是神明降下的天罚",
  "DS: 链表是首尾相连的死者之脉",
  "DS: 栈是先进后出的窒息囚笼",
  "DS: 队列是走向祭坛的无尽长龙",
  "DS: 二叉树不断分裂，长出非欧几何的枝丫",
  "DS: 图论遍历 (DFS/BFS) 是在迷宫中寻找出口的疯狂",
  "DS: 哈希表将灵魂映射到虚无的槽位 (Hash Collision)",
  "DS: 快速排序通过不断撕裂数组来重建秩序",
  "DS: 动态规划 (DP) 是窥探未来所有时间线的邪术",
  "DS: 最小生成树 (Prim/Kruskal) 连接破碎的世界",
  "DS: 拓扑排序决定了因果链条的先后顺序",
  "DS: KMP 算法在浩瀚的文本中精准定位灵魂印记",
  
  // --- 408 计算机基础 (根基) ---
  "408: 冯·诺依曼架构是禁锢算力的物理牢笼",
  "408: CPU 流水线 (Pipeline) 吞噬着时钟周期",
  "408: Cache 命中率决定了灵魂读取的延迟",
  "408: 虚拟内存 (Virtual Memory) 欺骗了进程的感知",
  "408: 缺页中断 (Page Fault) 是现实与虚幻的碰撞",
  "408: 进程控制块 (PCB) 记录了生命的生与死",
  "408: 死锁 (Deadlock) 是四个维度互相锁死的绝境",
  "408: 信号量 (Semaphore) 控制着争夺血肉的恶鬼",
  "408: 银行家算法 (Banker's) 试图在崩溃边缘寻找安全序列",
  "408: OSI 七层模型是凡人构建的巴别塔",
  "408: TCP 三次握手是跨越深渊的契约缔结",
  "408: 四次挥手是切断因果的最后诀别",
  "408: 拥塞控制 (Congestion) 防止信息洪流冲垮现实",
  "408: DNS 解析将不可名状的 IP 转化为凡人的真名",
  "408: CSMA/CD 在混沌的以太网中倾听碰撞的回音",
  
  // --- 高阶数学 (虚空) ---
  "MATH: 极限 (Limit) 永远逼近，却永远无法触碰真理",
  "MATH: 导数 (Derivative) 瞬间撕裂了平滑的现实曲线",
  "MATH: 洛必达法则 (L'Hôpital) 在 0/0 的混沌中寻找出口",
  "MATH: 多重积分吞噬了整个高维度的体积",
  "MATH: 泰勒展开 (Taylor Series) 将有限的肉身拆解为无限的碎片",
  "MATH: 微分方程描述了灵魂衰变的必然规律",
  "MATH: 偏导数寻找着各个维度的畸变梯度",
  "MATH: 傅里叶变换将时间的混沌转化为频率的秩序",
  "MATH: 拉普拉斯算子揭示了虚空场的散度",
  "MATH: 矩阵乘法 (Matrix Multiplication) 是空间维度的暴力扭曲",
  "MATH: 特征值与特征向量是物体在扭曲中不变的真实骨架",
  "MATH: 行列式 (Determinant) 衡量了维度坍缩的体积",
  "MATH: 概率密度函数勾勒出命运的不确定性轮廓",
  "MATH: 大数定律在无尽的混乱中孕育出必然的结果",
  
  // --- 考研英语 (咒语) ---
  "ENG: 长难句 (Long Sentences) 是绕乱理智的迷宫",
  "ENG: 虚拟语气 (Subjunctive) 描绘了未曾发生的时间线",
  "ENG: 词根词缀 (Roots & Affixes) 是拆解外神真名的手术刀",
  "ENG: 完形填空 (Cloze) 填补被岁月侵蚀的记忆空洞",
  "ENG: 倒装句 (Inversion) 颠倒了主谓的因果关系",
  "ENG: 独立主格 (Absolute Phrase) 脱离了主句的物理束缚",
  "ENG: 隐喻 (Metaphor) 将不可视之物具象化为凡人概念"
]

// ============================================================================
// [卷三：数据持久化与数据库钩子预留]
// 封装 RitualStorage，目前走 LocalStorage，为将来 Node+MongoDB 铺路
// ============================================================================
class RitualStorage {
  static get(key, def) {
    return Number(localStorage.getItem(`v12_final_${key}`)) || def
  }
  static set(key, val) {
    localStorage.setItem(`v12_final_${key}`, val)
  }
  static getStr(key, def) {
    return localStorage.getItem(`v12_final_${key}`) || def
  }
  static setStr(key, val) {
    localStorage.setItem(`v12_final_${key}`, val)
  }
  
  // [数据库异步同步钩子]
  // 供未来连接后端 API 使用
  static async syncToDB(payload) {
    console.log("[阿卡夏接口] 尝试连接远端数据库...", payload)
    /* try {
        const response = await fetch('https://your-api.com/api/v1/sync', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });
        return await response.json();
      } catch (err) {
        console.error("数据库连接被虚空阻断:", err);
      }
    */
  }
}

// ============================================================================
// [卷四：状态机与响应式数据中心]
// ============================================================================
const vocabSiphoned = ref(RitualStorage.get('vocab', 2150)) // 词汇量 (主要进度)
const karmaWeight = ref(RitualStorage.get('karma', 23.7))   // 业障
const sanityIndex = ref(RitualStorage.get('sanity', 99.9998)) // 理智 SAN
const deathCount = ref(RitualStorage.get('deaths', 0)) // 死亡回归次数
const caffeineLvl = ref(200) // 咖啡因浓度

// 四大柱神进度 (基于词汇量或时间推演，此处为示例绑定)
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

// 供奉残卷 (留言板数据)
// 初始化时读取本地存储，未来可从数据库拉取
const storedMessages = JSON.parse(RitualStorage.getStr('messages', '[]'))
const defaultMessages = [
  { text: "22408 终将飞升，血肉苦弱...", style: "transform: rotate(-3deg); opacity: 0.85; top: 10%; left: 5%;" },
  { text: "英语的咒语太难解析了", style: "transform: rotate(2deg); opacity: 0.6; top: 35%; left: 15%;" },
  { text: "408 的网络层让我理智耗尽", style: "transform: rotate(-1deg); opacity: 0.7; top: 60%; left: 10%;" }
]
const messageBoard = reactive(storedMessages.length > 0 ? storedMessages : defaultMessages)

// 计算：吞噬百分比 (嵌入瞳孔的核心数值)
const vocabProgressPercent = computed(() => {
  return Math.floor((vocabSiphoned.value / 4000) * 100)
})

// 计算：理智评级 (CSS 绑定)
const sanityStageClass = computed(() => {
  if (sanityIndex.value > 80) return 'san-safe'
  if (sanityIndex.value > 40) return 'san-warn'
  return 'san-broken'
})

// === 系统时间获取 ===
const getSysTime = () => {
  const d = new Date()
  return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}`
}

// === 日志推送系统 ===
const pushLog = (msg, type = 'info') => {
  logs.push({ time: getSysTime(), msg, type })
  if (logs.length > 80) logs.shift()
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

// === 多维链接拦截器 ===
const invokeLink = (dimension) => {
  pushLog(`尝试开启维度裂隙 [${dimension}] ...`, 'sys')
  setTimeout(() => {
    pushLog(`维度跃迁失败：目标锚点尚未在现实中构筑。`, 'error')
    karmaWeight.value += 0.2; RitualStorage.set('karma', karmaWeight.value)
  }, 800)
}

// ============================================================================
// [卷五：祭坛交互终端 (CLI Ritual Parser)]
// ============================================================================
const handleSacrifice = () => {
  const inputStr = rawInput.value.trim()
  if (!inputStr) return
  pushLog(`> ${inputStr}`, 'echo')
  
  // 提取命令和参数
  const args = inputStr.split(' ')
  const cmd = args[0].toLowerCase()

  switch (cmd) {
    case 'vocab':
      const amt = parseInt(args[1])
      if (amt > 0) {
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
        RitualStorage.set('vocab', vocabSiphoned.value)
        
        // 考研进步，洗刷业障，提升英语柱神
        karmaWeight.value = Math.max(0, karmaWeight.value - (amt * 0.05))
        RitualStorage.set('karma', karmaWeight.value)
        progressEng.value = Math.min(100, progressEng.value + (amt * 0.05))
        RitualStorage.set('prog_eng', progressEng.value)
        
        sanityIndex.value = Math.min(100, sanityIndex.value + 0.2)
        RitualStorage.set('sanity', sanityIndex.value)
        
        pushLog(`灵魂吞噬完成。同化率上升，当前进度: ${vocabProgressPercent.value}%`, 'success')
        RitualStorage.syncToDB({ type: 'progress_update', value: vocabSiphoned.value })
      } else pushLog('献祭失败：虚无的供品。', 'error')
      break

    case 'dose':
      const dose = parseInt(args[1])
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
      }
      break

    case 'msg':
      // 提取 msg 之后的所有内容作为留言
      const text = inputStr.substring(4).trim()
      if (text.length > 0) {
        // 随机生成掉落位置和倾斜度
        const rot = (Math.random() - 0.5) * 10
        const top = Math.random() * 70 + 10
        const left = Math.random() * 20 + 5
        const op = Math.random() * 0.3 + 0.7
        
        const newMsg = {
          text: text,
          style: `transform: rotate(${rot}deg); top: ${top}%; left: ${left}%; opacity: ${op}; z-index: ${Date.now()};`
        }
        
        messageBoard.push(newMsg)
        // 保持最多 10 条，避免重叠过度
        if(messageBoard.length > 10) messageBoard.shift()
        
        RitualStorage.setStr('messages', JSON.stringify(messageBoard))
        pushLog(`供奉低语已刻印入残卷。`, 'success')
        RitualStorage.syncToDB({ type: 'new_message', text: text })
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
        sanityIndex.value = 100 // 理智重置
        karmaWeight.value += 10.0 // 业障累加
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
// 核心任务：
// 1. 物理移除所有 JPG 白底 (ctx.globalCompositeOperation = 'multiply')
// 2. 凝视眼球 (鼠标追踪，渲染 53% 在瞳孔)
// 3. 考研咒语坠落 (万有引力)
// 4. IK 触手动力学 (反向运动学)
// 5. 虚空墨水升腾
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

// === 物理类 1：坠落的考研知识符文 (Falling Arcane Debris) ===
class FallingRune {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = Math.random() * this.w
    this.y = - (Math.random() * 800 + 100) // 从极高处坠落
    this.vx = (Math.random() - 0.5) * 1.2
    this.vy = Math.random() * 2 + 0.8 // 基础重力
    this.angle = Math.random() * Math.PI * 2
    this.av = (Math.random() - 0.5) * 0.04 // 角速度
    this.text = arcaneLoreDatabase[Math.floor(Math.random() * arcaneLoreDatabase.length)]
    this.size = Math.random() * 6 + 12
    this.opacity = Math.random() * 0.5 + 0.2
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
    ctx.globalCompositeOperation = 'multiply' // 烧录
    ctx.font = `italic ${this.size}px 'Space Mono', 'Microsoft YaHei', monospace`
    ctx.fillStyle = `rgba(13, 20, 15, ${this.opacity})` // 极深墨色
    ctx.fillText(this.text, 0, 0)
    ctx.restore()
  }
}

// === 物理类 2：业障灰烬升腾粒子 (Floating Karma Ash) ===
class FloatingAsh {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = (this.w / 2) + (Math.random() - 0.5) * 600
    this.y = this.h + Math.random() * 200
    this.vx = (Math.random() - 0.5) * 1.5
    this.vy = - (Math.random() * 2 + 1) // 向上浮动
    this.life = Math.random() * 200 + 100; this.maxLife = this.life
    this.r = Math.random() * 2.5 + 0.5
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

// === 物理类 3：反向运动学 (IK) 触手 ===
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
      const wob = Math.sin(time * (hyper ? 6 : 2) + i * 0.4 + this.noise) * (hyper ? 6 : 2)
      ctx.lineTo(this.joints[i].x + wob, this.joints[i].y + wob)
    }
    ctx.strokeStyle = hyper ? 'rgba(139, 0, 0, 0.5)' : this.color
    ctx.lineWidth = hyper ? 4 : 2; ctx.lineCap = 'round'; ctx.stroke()
  }
}

// === 核心机制：绝对凝视眼球 (The Observer Eye) ===
// 取缔了旧的黑水进度条。现在进度百分比写在追随鼠标的瞳孔中。
class OmniscientEye {
  constructor(w, h) {
    this.w = w; this.h = h
    this.radius = 80 // 眼白半径
    this.pupilR = 35 // 瞳孔半径
    this.cx = w / 2; this.cy = h * 0.78 // 锚定在耶稣正下方
    this.px = this.cx; this.py = this.cy // 瞳孔当前位置
    this.blinkTime = Math.random() * 200 + 100
  }
  
  update(mx, my) {
    // 追踪鼠标计算
    const dx = mx - this.cx; const dy = my - this.cy
    const dist = Math.sqrt(dx * dx + dy * dy)
    const maxOffset = this.radius - this.pupilR - 8 // 不出眼白
    
    if (dist > 0) {
      this.px = this.cx + (dx / dist) * Math.min(dist * 0.15, maxOffset)
      this.py = this.cy + (dy / dist) * Math.min(dist * 0.15, maxOffset)
    } else {
      this.px = this.cx; this.py = this.cy
    }
    
    this.blinkTime--
    if (this.blinkTime < -5) this.blinkTime = Math.random() * 200 + 100
  }
  
  draw(ctx, percent, hyper) {
    ctx.save()
    // 1. 绘制眼白基底 (SourceOver)
    ctx.beginPath()
    ctx.arc(this.cx, this.cy, this.radius, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(235, 232, 222, 0.95)' // 浑浊感
    ctx.fill()
    ctx.lineWidth = 3; ctx.strokeStyle = 'rgba(20, 20, 20, 0.9)'; ctx.stroke()
    
    // 2. 绘制血丝 (向心辐射)
    for(let i=0; i<36; i++) {
        const ang = Math.random() * Math.PI * 2
        const sR = this.pupilR + Math.random() * 15
        const eR = this.radius - Math.random() * 5
        ctx.beginPath()
        ctx.moveTo(this.cx + Math.cos(ang)*sR, this.cy + Math.sin(ang)*sR)
        ctx.lineTo(this.cx + Math.cos(ang)*eR, this.cy + Math.sin(ang)*eR)
        ctx.strokeStyle = `rgba(139, 0, 0, ${Math.random()*0.3 + 0.1})`
        ctx.lineWidth = Math.random() > 0.8 ? 1 : 0.5
        ctx.stroke()
    }

    // 3. 绘制瞳孔与嵌入数字
    if (this.blinkTime > 0) {
      // 瞳孔
      ctx.beginPath()
      ctx.arc(this.px, this.py, this.pupilR, 0, Math.PI * 2)
      ctx.fillStyle = hyper ? '#500' : '#111'
      ctx.fill()
      
      // 53% 嵌入
      ctx.fillStyle = '#E2DED0'
      ctx.font = `bold 34px 'Cinzel', serif`
      ctx.textAlign = 'center'; ctx.textBaseline = 'middle'
      ctx.fillText(`${percent}%`, this.px, this.py + 2)
      
      // 高光
      ctx.beginPath(); ctx.arc(this.px - 12, this.py - 12, 5, 0, Math.PI*2)
      ctx.fillStyle = 'rgba(255,255,255,0.5)'; ctx.fill()
    } else {
      // 眨眼闭合
      ctx.beginPath()
      ctx.moveTo(this.cx - this.radius, this.cy)
      ctx.bezierCurveTo(this.cx - 20, this.cy + 30, this.cx + 20, this.cy + 30, this.cx + this.radius, this.cy)
      ctx.lineWidth = 4; ctx.strokeStyle = '#111'; ctx.stroke()
    }
    
    ctx.restore()
  }
}

// --- 渲染主线程 ---
let engine, textures = {}, physicsArr = {}

const bootRenderMatrix = async () => {
  const cvs = mainCanvas.value
  ctx = cvs.getContext('2d')
  w = window.innerWidth; h = window.innerHeight
  cvs.width = w; cvs.height = h

  pushLog('连接底层阿卡夏资源库，正在加载真名材质...', 'sys')
  textures = {
    paper: await loadImage(imgPaper), totem: await loadImage(imgTotem),
    ink1: await loadImage(imgInk1), ink2: await loadImage(imgInk2), ink3: await loadImage(imgInk3),
    ink4: await loadImage(imgInk4), ink5: await loadImage(imgInk5),
    ten1: await loadImage(imgTen1), ten2: await loadImage(imgTen2)
  }
  pushLog('素材解析完成。Canvas 光学阵列已部署。白底死刑确认。', 'success')

  // 初始化引擎核心数据
  engine = {
    eye: new OmniscientEye(w, h),
    mouse: { x: w/2, y: h/2 },
    time: 0,
    hyper: false,
    animId: null
  }
  window.vzuorPhysicsEngine = engine

  // 初始化物理阵列
  physicsArr.runes = Array.from({length: 40}, () => new FallingRune(w, h))
  physicsArr.ashes = Array.from({length: 100}, () => new FloatingAsh(w, h))
  physicsArr.tentacles = [
    new IKTentacle(w*0.1, h, 18, 20, 'rgba(15, 15, 15, 0.4)'), // 左下
    new IKTentacle(w*0.9, h, 20, 22, 'rgba(15, 15, 15, 0.5)'), // 右下
    new IKTentacle(w/2, -50, 25, 25, 'rgba(40, 10, 10, 0.2)')  // 顶部垂下
  ]

  // 监听器
  window.addEventListener('mousemove', e => { engine.mouse.x = e.clientX; engine.mouse.y = e.clientY })
  window.addEventListener('resize', () => { 
    w = window.innerWidth; h = window.innerHeight; cvs.width = w; cvs.height = h
    engine.eye.w = w; engine.eye.h = h; engine.eye.cx = w/2; engine.eye.cy = h*0.78
    physicsArr.tentacles[0].bx = w*0.1; physicsArr.tentacles[0].by = h
    physicsArr.tentacles[1].bx = w*0.9; physicsArr.tentacles[1].by = h
    physicsArr.tentacles[2].bx = w/2; physicsArr.tentacles[2].by = -50
  })

  // 渲染循环 (The Render Grimoire)
  const renderLoop = () => {
    const { hyper, time, mouse, eye } = engine
    
    // 制造虚空拖影 (不透明底色覆盖)
    ctx.fillStyle = 'rgba(226, 222, 208, 0.3)'
    ctx.fillRect(0, 0, w, h)

    // [层 1：考研符文坠落] (自带 opacity)
    physicsArr.runes.forEach(r => { r.update(hyper); r.draw(ctx) })

    // [层 2：绝对正片叠底 - 物理抹杀所有 JPG 白底]
    ctx.globalCompositeOperation = 'multiply'
    ctx.globalAlpha = hyper ? 1.0 : 0.9

    // 绘制四周墨迹
    if(textures.ink1) ctx.drawImage(textures.ink1, -w*0.1, -h*0.1, w*0.4, h*0.5)
    if(textures.ink2) ctx.drawImage(textures.ink2, w*0.6, h*0.6, w*0.5, h*0.5)
    ctx.globalAlpha = 0.6
    if(textures.ink3) ctx.drawImage(textures.ink3, w*0.75, -h*0.05, w*0.3, h*0.4)
    if(textures.ink4) ctx.drawImage(textures.ink4, -w*0.05, h*0.6, w*0.3, h*0.4)
    
    // 绘制静默触手素描
    ctx.globalAlpha = hyper ? 0.35 : 0.25
    if(textures.ten2) ctx.drawImage(textures.ten2, w*0.05, h*0.15, w*0.2, h*0.3)

    // === 核心祭坛：耶稣受难 ===
    ctx.globalAlpha = 1.0
    const tH = h * 0.8
    const tW = tH * (textures.totem.width / textures.totem.height)
    const tx = (w/2 - tW/2) + (hyper ? (Math.random()-0.5)*12 : 0)
    const ty = h * 0.02 + (hyper ? (Math.random()-0.5)*12 : 0)
    ctx.drawImage(textures.totem, tx, ty, tW, tH)

    // [层 3：恢复正常模式，绘制实体对象]
    ctx.globalCompositeOperation = 'source-over'

    // 动态 IK 触手追踪鼠标
    physicsArr.tentacles.forEach((t, i) => {
      const tgX = hyper ? w/2 + Math.cos(time*4+i)*500 : mouse.x + Math.cos(time+i)*60
      const tgY = hyper ? h/2 + Math.sin(time*3+i)*500 : mouse.y + Math.sin(time+i)*60
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
  pushLog('V12 观察者协议引序列启动。', 'sys')
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
    
    // SAN 流失计算
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
  --f-whisper: 'Zhi Mang Xing', cursive; /* 用于模拟留言板的手写狂草感 */
}

body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: var(--c-paper); color: var(--c-ink); font-family: var(--f-mono); }
.ascension-root { position: relative; width: 100vw; height: 100vh; }

/* 物理滤镜层 */
.occult-filters-defs { position: absolute; pointer-events: none; }
.san-warn { filter: contrast(1.1) sepia(0.1); }
.san-broken { filter: url(#flesh-melt) contrast(1.3) brightness(0.9); }

/* 底层材质 */
.global-parchment { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover; z-index: 0; }
.occult-noise-overlay { position: absolute; inset: 0; z-index: 1; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="4"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.05; }
.abyssal-vignette-mask { position: absolute; inset: 0; z-index: 2; pointer-events: none; background: radial-gradient(circle, transparent 40%, rgba(20,0,0,0.4) 100%); }

/* Canvas 渲染阵列 */
.abyssal-render-layer { position: absolute; inset: 0; z-index: 10; display: block; width: 100%; height: 100%; }

/* ============================================================================
   [无框嵌入式 UI 矩阵]
   使用 multiply 正片叠底，让所有的 DOM 文字像墨水一样渗入背景图
   ============================================================================ */
.embedded-ui-layer {
  position: relative; z-index: 100;
  width: 100%; height: 100%; padding: 2.5rem; box-sizing: border-box;
  pointer-events: none;
}

/* 碎片基础类：绝对不要写 background! */
.shard {
  position: absolute; pointer-events: auto;
  background: transparent !important; border: none !important; box-shadow: none !important; backdrop-filter: none !important;
  mix-blend-mode: multiply; /* 终极烧录奥义 */
}
.shard-title { font-size: 1.1rem; letter-spacing: 2px; border-bottom: 2px solid var(--c-ink); padding-bottom: 5px; margin: 0 0 1.2rem 0; font-weight: bold; }

/* 1. 天极命数 (左上) */
.shard-top-left { top: 3rem; left: 3rem; }
.artifact-title { font-family: var(--f-title); font-size: 3.5rem; font-weight: 900; margin: 0; letter-spacing: 2px; position: relative; }
.artifact-title::before { content: attr(data-text); position: absolute; left: -2px; text-shadow: 2px 0 red; opacity: 0.5; animation: glitch 3s infinite linear alternate-reverse; }
.doom-clock-wrap { margin-top: 15px; }
.clock-num { font-size: 2.8rem; font-weight: 700; letter-spacing: -2px; line-height: 1; }
.clock-label { font-size: 0.8rem; font-weight: bold; color: var(--c-text-muted); margin-top: 5px; }

/* 2. 左侧超链接浮窗 (左中) */
.shard-left-float { top: 40%; left: 3rem; transform: translateY(-50%); width: 250px; }
.nav-links { display: flex; flex-direction: column; gap: 15px; }
.drift-link { color: var(--c-ink); text-decoration: none; font-weight: bold; font-size: 1.1rem; transition: all 0.3s ease; position: relative; }
.drift-link:hover { color: var(--c-blood); transform: translateX(10px) scale(1.05); text-shadow: 0 0 5px rgba(139,0,0,0.5); }
.drift-link::before { content: ''; position: absolute; left: -15px; top: 50%; width: 5px; height: 5px; background: var(--c-blood); border-radius: 50%; opacity: 0; transition: 0.3s; }
.drift-link:hover::before { opacity: 1; }

/* 3. 灵魂与肉身刻度 (左下 - 包含英语) */
.shard-bottom-left { bottom: 3rem; left: 3rem; width: 340px; }
.metrics-list { display: flex; flex-direction: column; gap: 12px; font-size: 0.95rem; font-weight: bold; }
.m-item { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px dashed rgba(0,0,0,0.15); padding-bottom: 4px; }
.text-blood { color: var(--c-blood); }
.hyper { color: #cc0000; font-size: 1.15rem; text-shadow: 0 0 10px rgba(200,0,0,0.8); animation: jitter 0.1s infinite; }

.pillar-matrix { margin-top: 1.8rem; font-size: 0.85rem; font-weight: bold; display: flex; flex-direction: column; gap: 10px; }
.p-unit { display: flex; flex-direction: column; gap: 6px; }
.p-bar { width: 100%; height: 6px; background: rgba(0,0,0,0.15); border: 1px solid var(--c-ink); }
.p-fill { height: 100%; background: var(--c-ink); transition: width 1s ease; }

/* 4. 供奉残卷 - 留言板 (右中) */
.shard-right-mid { top: 40%; right: 3rem; transform: translateY(-50%); width: 350px; }
.offering-scraps-container { position: relative; height: 280px; width: 100%; }
.scrap-whisper { 
  position: absolute; 
  font-family: var(--f-whisper); /* 手写狂草字体 */
  font-size: 1.8rem; color: #1a1a1a; 
  line-height: 1.2; text-shadow: 0 0 2px rgba(0,0,0,0.2); 
  width: 80%; pointer-events: none;
}

/* 5. 阿卡夏终端 (右下) */
.shard-bottom-right { bottom: 3rem; right: 3rem; width: 450px; }
.terminal-box { display: flex; flex-direction: column; gap: 10px; }
.terminal-scroll-area { height: 180px; overflow-y: auto; font-size: 0.75rem; display: flex; flex-direction: column; gap: 6px; background: rgba(0,0,0,0.02); padding: 10px; border-left: 2px solid var(--c-ink); }
.terminal-scroll-area::-webkit-scrollbar { width: 3px; }
.terminal-scroll-area::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.3); }
.log-line { line-height: 1.4; }
.log-line .ts { color: var(--c-text-muted); margin-right: 5px; opacity: 0.8; }
.log-line.sys { color: #444; font-style: italic; }
.log-line.echo { color: #666; font-style: italic; }
.log-line.success { color: var(--c-terminal); font-weight: bold; text-shadow: 0 0 2px rgba(0,255,102,0.5); }
.log-line.error, .log-line.alert { color: var(--c-blood); font-weight: bold; }

/* 这里输入框为了可视性，局部取消 multiply，使用 rgba 背景 */
.cli-input-wrapper { display: flex; align-items: center; gap: 10px; background: rgba(226, 222, 208, 0.85); padding: 12px 20px; border: 2px solid var(--c-ink); border-radius: 4px; mix-blend-mode: normal; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }
.cli-prompt { font-weight: 900; font-size: 1.2rem; color: var(--c-terminal); text-shadow: 1px 1px 0 #000; }
.cli-input-field { flex: 1; border: none; background: transparent; outline: none; font-size: 1.3rem; font-family: inherit; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed rgba(0,0,0,0.3); transition: 0.3s; }
.cli-input-field:focus { border-bottom: 1px solid var(--c-ink); }
.cli-input-field::placeholder { color: #777; font-size: 0.9rem; font-style: normal; }
.cli-input-field:disabled { opacity: 0.3; cursor: not-allowed; }

/* 死亡回归 */
.death-return-shroud { position: fixed; inset: 0; z-index: 9999; background: #050505; display: flex; justify-content: center; align-items: center; }
.death-core { text-align: center; }
.death-words { color: #fff; font-family: var(--f-title); font-size: 4rem; letter-spacing: 8px; margin: 0; }
.death-sub { color: #888; font-size: 1.2rem; letter-spacing: 4px; margin-top: 20px; animation: pulse 1s infinite; }
.glitch-text { animation: violent-shake 0.1s infinite; }

/* 动画库 */
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes violent-shake { 0% { transform: translate(3px, 2px) rotate(1deg); } 50% { transform: translate(-2px, 3px) rotate(-1deg); } 100% { transform: translate(2px, -2px) rotate(1deg); } }
@keyframes pulse { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
@keyframes glitch { 0% { clip: rect(10px, 9999px, 30px, 0); } 50% { clip: rect(60px, 9999px, 80px, 0); } 100% { clip: rect(20px, 9999px, 50px, 0); } }
</style>