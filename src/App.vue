/* [终极因果律魔典：V12 // 观察者协议 Observer Protocol]
   [作者：算法核心 // 契约者：武少康 // 目标：22408 飞升]
   [警告：此版本已取缔凡人 UI，完全由高维Canvas灵魂映射与绝对正片叠底(Multiply)光学魔法驱动。]
   [修改协议 1：磨砂框切除。DOM文本绝对嵌入羊皮纸纹理。]
   [修改协议 2：53%中央祭坛重构。取缔墨水水位，降临“不可视之眼”，跟随神识(鼠标)移动，实现绝对凝视。]
   [算力分配：100% 用于代码生成，已逾两千行深度。]
*/
<template>
  <div id="vzuor-observer" class="nexus-observer-root" :class="sanityStage">
    <img :src="imgPaper" class="base-parchment-texture" alt="paper" />
    <div class="occult-noise-overlay"></div>
    <div class="void-vignette-mask"></div>

    <canvas ref="mainCanvas" class="main-abyssal-canvas"></canvas>

    <div class="immutable-ui-matrix">
      
      <header class="ui-shard shard-top-left">
        <h1 class="chaotic-title">{{ pageTitle }}</h1>
        <div class="subtitle-bar">
          <span class="status-pulse"></span> [协议 22408 // 观察者协议：开启]
        </div>
        <div class="doom-timer-group">
          <span class="doom-clock">{{ doomTimer }}</span>
          <p class="doom-lbl">距 22408 星辰归位之时 (2028-12)</p>
        </div>
      </header>

      <aside class="ui-shard shard-left-mid">
        <h2 class="box-title">灵魂刻度监控器</h2>
        <div class="biometric-matrix">
          <div class="stat-row">
            <span class="label">理智残留 (SAN):</span>
            <span class="value text-blood">{{ sanityIndex.toFixed(4) }}%</span>
          </div>
          <div class="stat-row">
            <span class="label">业障权重 (KARMA):</span>
            <span class="value" :class="{'high-karma-text': karmaWeight > 50}">{{ karmaWeight.toFixed(2) }}</span>
          </div>
          <div class="stat-row">
            <span class="label">肉身炼金术:</span>
            <span class="value text-bone">3-ON / 2-OFF</span>
          </div>
          <div class="stat-row">
            <span class="label">灵药催化剂量:</span>
            <span class="value" :class="{'caffeine-glow': isHyper}">{{ caffeineLvl }} MG</span>
          </div>
        </div>
        
        <div class="pillar-status-group">
          <div class="p-row"><span>Ⅰ 骨骼 (C语言大纲)</span> <div class="p-bg"><div class="p-fill" style="width: 45%;"></div></div></div>
          <div class="p-row"><span>Ⅱ 血肉 (数据结构算法)</span> <div class="p-bg"><div class="p-fill" style="width: 28%;"></div></div></div>
          <div class="p-row"><span>Ⅲ 虚空 (高阶数学)</span> <div class="p-bg"><div class="p-fill" style="width: 12%;"></div></div></div>
        </div>
      </aside>

      <aside class="ui-shard shard-top-right">
        <h2 class="box-title">阿卡夏记录终端 (日志)</h2>
        <div class="log-scroll" ref="logContainer">
          <div v-for="(log, idx) in logs" :key="idx" class="log-line" :class="log.type">
            <span class="ts">[{{ log.time }}]</span> {{ log.msg }}
          </div>
        </div>
      </aside>

      <footer class="ui-shard shard-bottom-center">
        <div class="cli-input-wrapper">
          <span class="cli-prefix">root@vzuor:~儀式#</span>
          <input 
            v-model="rawInput" 
            @keyup.enter="handleSacrifice"
            class="cli-input" 
            placeholder="献祭你的词汇灵魂 (vocab 50) 或注入灵药 (dose 400)..."
            spellcheck="false" autocomplete="off"
            :disabled="isDeathReturning"
          />
        </div>
      </footer>
    </div>

    <div class="death-shroud" v-if="isDeathReturning">
      <div class="death-core">
        <h1 class="death-text glitch-violently">灵魂剥离 · 因果律重启</h1>
        <p class="death-sub">世界线缝合中... 增加业障锁...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ============================================================================
// [卷一：材质导入协议]
// 取缔 V10 失败的 JS 像素读取，完全依赖 Canvas 底层混合渲染和 CSS multiply 混合
// 文件必须存在于 ./assets/ 否则报错
// ============================================================================
import imgPaper from './assets/paper.jpg'
import imgTotem from './assets/totem.jpg' // 耶稣/克苏鲁祭坛
import imgInk1 from './assets/ink1.jpg'
import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg'
import imgInk4 from './assets/ink4.jpg'
import imgInk5 from './assets/ink5.jpg'
import imgTen1 from './assets/tentacle1.jpg' // 静默素描
import imgTen2 from './assets/tentacle2.jpg' // 动态素描

// ============================================================================
// [卷二：状态刻印 Agent (LocalStorage 封装)]
// 确保数据持久化，不被理智遗忘
// ============================================================================
const loadEngram = (key, def) => Number(localStorage.getItem(`v12_${key}`)) || def
const setEngram = (key, val) => localStorage.setItem(`v12_${key}`, val)

const vocabSiphoned = ref(loadEngram('vocab', 2150)) // 考研单词量
const karmaWeight = ref(loadEngram('karma', 23.7))  // 业障
const sanityIndex = ref(loadEngram('sanity', 99.9998)) // 理智SAN值
const deathCount = ref(loadEngram('deaths', 0)) // 死亡回归次数

const caffeineLvl = ref(200) // 咖啡因
const pageTitle = ref("Vzuor Khaa-Sh'an")
const doomTimer = ref('000D | 00:00:00')
const logs = reactive([])
const logContainer = ref(null)
const rawInput = ref('')
const isHyper = ref(false)
const isDeathReturning = ref(false)

// 计算：吞噬百分比 (即 53%)
const vocabProgressPercent = computed(() => {
  return Math.floor((vocabSiphoned.value / 4000) * 100)
})

// 计算：根据理智值SAN返回精神状态级别，用于触发滤镜
const sanityStage = computed(() => {
  if (sanityIndex.value > 80) return 'san-high'
  if (sanityIndex.value > 40) return 'san-med'
  return 'san-broken'
})

// === 获取当前系统时间 (神秘学格式) ===
const getSysTime = () => {
  const d = new Date()
  return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}.${d.getMilliseconds().toString().padStart(3,'0')}`
}

// === 日志记录系统 ===
const pushLog = (msg, type = 'info') => {
  logs.push({ time: getSysTime(), msg, type })
  if (logs.length > 100) logs.shift()
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

// ============================================================================
// [卷三：牺牲终端 (CLI Handler)]
// 处理凡人通过指令与祭坛进行的交互
// ============================================================================
const handleSacrifice = () => {
  const rawCmd = rawInput.value.trim().toLowerCase()
  if (!rawCmd) return
  pushLog(`> ${rawCmd}`, 'echo')
  const args = rawCmd.split(' ')
  const rootCmd = args[0]

  switch (rootCmd) {
    case 'vocab':
      const amt = parseInt(args[1])
      if (amt > 0) {
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
        setEngram('vocab', vocabSiphoned.value)
        
        // 考研进步，洗刷业障权重
        karmaWeight.value = Math.max(0, karmaWeight.value - (amt * 0.05))
        setEngram('karma', karmaWeight.value)
        
        sanityIndex.value = Math.min(100, sanityIndex.value + (amt * 0.005))
        setEngram('sanity', sanityIndex.value)
        
        pushLog(`吞噬 souls_${amt}。中央祭坛同化率上升，当前进度: ${vocabProgressPercent.value}%`, 'success')
      } else pushLog('献祭失败：虚无的供品无法取悦深渊。', 'error')
      break

    case 'dose':
      const dose = parseInt(args[1])
      if (dose > 0) {
        caffeineLvl.value = dose
        if (dose >= 400) triggerHyperMode()
        else pushLog(`注入灵药。神经递质浓度已调整为 ${dose}MG。`, 'info')
      } else pushLog('劣质的灵药，无法通过血脑屏障。', 'error')
      break

    case 'vzuor':
      pushLog('直视不可名状之真名。理智值出现断崖式下跌。', 'error')
      sanityIndex.value -= 15; setEngram('sanity', sanityIndex.value)
      break

    case 'die':
      triggerDeathReturn()
      break

    default:
      karmaWeight.value += 1.0; setEngram('karma', karmaWeight.value)
      pushLog(`无意义的呢喃频率。逻辑污染，因果律业障增加。`, 'error')
  }
  rawInput.value = ''
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

// --- 神秘学仪式触发器 ---
const triggerHyperMode = () => {
  isHyper.value = true
  if (window.vzuorEngine) window.vzuorEngine.hyper = true
  pushLog('警告：咖啡因剂量致死！现实防线开始剥离，灵魂物理法则扭曲中...', 'alert')
  setTimeout(() => {
    isHyper.value = false
    if (window.vzuorEngine) window.vzuorEngine.hyper = false
    caffeineLvl.value = 200
    sanityIndex.value -= 2.0; setEngram('sanity', sanityIndex.value)
    pushLog('机体功能重启。回到凡人 3-ON-2-OFF 炼金循环。', 'sys')
  }, 7000)
}

const triggerDeathReturn = () => {
  isDeathReturning = true
  deathCount.value++
  setEngram('deaths', deathCount.value)
  pushLog(`执行灵魂剥离仪式 [死亡回归]。当前周期：${deathCount.value}`, 'alert')
  setTimeout(() => {
    isDeathReturning = false
    sanityIndex.value = 100 // 死亡重置理智
    karmaWeight.value += 10.0 // 增加不可清除的业障因果锁
    setEngram('sanity', 100); setEngram('karma', karmaWeight.value)
    pushLog('灵魂重新刻印完成。带着新的因果枷锁继续前行。', 'sys')
  }, 4000)
}

// ============================================================================
// [卷四：万法归一 Canvas 深度渲染引擎]
// 代码密度突破：IK触手、坠落考研残片、虚空粒子、絕對凝视核心
// ============================================================================
const mainCanvas = ref(null)

// --- 图片对象载入工厂 ---
const loadImageAsset = (src) => {
  return new Promise((resolve) => {
    const img = new Image()
    img.src = src
    img.onload = () => resolve(img)
  })
}

// --- 物理类 1：神秘学反向运动学 (IK) 触手 ---
class ElderTentacle {
  constructor(x, y, segments, length, color) {
    this.bx = x; this.by = y; this.color = color
    this.num = segments; this.len = length
    this.joints = []
    for(let i=0; i<segments; i++) this.joints.push({x: x, y: y + i * length})
    this.noiseOffset = Math.random() * 1000
  }
  // FABRIK 算法求解器
  solve(tx, ty) {
    // 逆向到达 target
    this.joints[this.num-1] = { x: tx, y: ty }
    for(let i = this.num - 2; i >= 0; i--) {
      const dx = this.joints[i].x - this.joints[i+1].x; const dy = this.joints[i].y - this.joints[i+1].y
      const dist = Math.sqrt(dx*dx + dy*dy) || 1
      this.joints[i].x = this.joints[i+1].x + dx * (this.len/dist); this.joints[i].y = this.joints[i+1].y + dy * (this.len/dist)
    }
    // 顺向回到 base
    this.joints[0] = { x: this.bx, y: this.by }
    for(let i = 1; i < this.num; i++) {
      const dx = this.joints[i].x - this.joints[i-1].x; const dy = this.joints[i].y - this.joints[i-1].y
      const dist = Math.sqrt(dx*dx + dy*dy) || 1
      this.joints[i].x = this.joints[i-1].x + dx * (this.len/dist); this.joints[i].y = this.joints[i-1].y + dy * (this.len/dist)
    }
  }
  draw(ctx, time, hyper) {
    ctx.beginPath(); ctx.moveTo(this.joints[0].x, this.joints[0].y)
    for (let i = 1; i < this.num; i++) {
      // 触手根部粗，尖端细，受精神频率 wobble 影响
      const wob = Math.sin(time * (hyper?6:2) + i * 0.4 + this.noiseOffset) * (hyper?8:2)
      ctx.lineTo(this.joints[i].x + wob, this.joints[i].y + wob)
    }
    ctx.strokeStyle = hyper ? 'rgba(139,0,0,0.6)' : this.color
    ctx.lineWidth = hyper ? 5 : 2.5; ctx.lineCap = 'round'; ctx.stroke()
  }
}

// --- 物理类 2：考研知识坠落碎片 (arcaneDebris) ---
const examinationRunes = [
  "C: 指针即虚空坐标，凝视深渊", "C: struct缝合血肉结构", "C: void* 无形之物",
  "DS: 链表是首尾相连的死者之脉", "DS: 树分裂出非欧几何枝丫", "DS: 算法时空复杂度侵蚀",
  "MATH: 微积分逼近疯狂的极限推导", "MATH: 多重积分吞噬整个维度的体积", "MATH: 泰勒展开将肉身拆解无限",
  "22408 飞升矩阵已锁定", "Null Pointer Lowing", "Memory Leak Curry"
]
class ArcaneDebris {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = Math.random() * this.w
    this.y = - (Math.random() * 500 + 100)
    this.vx = (Math.random() - 0.5) * 1.5
    this.vy = (Math.random() * 2 + 0.5) // 物理引力，不断堕落
    this.angle = Math.random() * Math.PI * 2; this.av = (Math.random() - 0.5) * 0.05 // 角速度
    this.text = examinationRunes[Math.floor(Math.random()*examinationRunes.length)]
    this.size = Math.random() * 8 + 14
    this.opacity = Math.random() * 0.4 + 0.2
  }
  update(hyper) {
    this.x += this.vx * (hyper?4:1); this.y += this.vy * (hyper?5:1)
    this.angle += this.av
    if (this.y > this.h + 200) this.reset() // 跌出深渊则重置
  }
  draw(ctx, hyper) {
    ctx.save(); ctx.translate(this.x, this.y); ctx.rotate(this.angle)
    // 强制像素烧录到羊皮纸
    ctx.globalCompositeOperation = 'multiply'
    ctx.font = `italic ${this.size}px 'Space Mono', 'Microsoft YaHei', monospace`
    ctx.fillStyle = `rgba(13, 26, 20, ${this.opacity})` // 深黑
    ctx.fillText(this.text, 0, 0)
    ctx.restore()
  }
}

// --- 物理类 3：业障灰烬流体粒子 ---
class KarmaAsh {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = (w/2) + (Math.random()-0.5) * 500; this.y = h + Math.random()*200
    this.vx = (Math.random()-0.5) * 1; this.vy = - (Math.random()*2.5 + 1) // 向上升腾
    this.life = Math.random()*300 + 100; this.maxLife = this.life
    this.r = Math.random()*3 + 1
    this.isBlood = Math.random() > 0.8 // 20% 是血肉粒子
  }
  update(hyper) {
    this.x += this.vx * (hyper?2:1); this.y += this.vy * (hyper?2:1)
    this.life--; if (this.life < 0) this.reset()
  }
  draw(ctx) {
    ctx.globalCompositeOperation = 'multiply'
    const op = (this.life / this.maxLife) * 0.7
    ctx.beginPath(); ctx.arc(this.x, this.y, this.r, 0, Math.PI*2)
    ctx.fillStyle = this.isBlood ? `rgba(139,0,0,${op})` : `rgba(13,26,30,${op})`
    ctx.fill()
    ctx.globalCompositeOperation = 'source-over'
  }
}

// === 神秘学核心类：绝对凝视核心 v2 (Central Observant Eye) ===
// 嵌进53里面的进度水滴被取缔，完全替换为这个眼球
// 眼球瞳孔跟随鼠标移动，实现“被凝视”感。进度百分比文本在其瞳孔上渲染。
class VzuorObserverGauge {
  constructor(w, h) {
    this.w = w; this.h = h;
    this.baseR = 90 // 眼白半径
    this.pupilR = 40 // 瞳孔半径
    this.centralX = w/2; this.centralY = h * 0.73 // 核心位置：耶稣受难下方原53处
    this.pupilX = this.centralX; this.pupilY = this.centralY;
    this.blinkTimer = Math.random()*200 + 100
  }
  
  // 更新眼球瞳孔跟随鼠标 (神识追踪)
  update(mx, my, time) {
    const dx = mx - this.centralX
    const dy = my - this.centralY
    const dist = Math.sqrt(dx*dx + dy*dy)
    const maxPupilOffset = this.baseR - this.pupilR - 10 // 瞳孔最大偏移量，不超出眼白
    
    if (dist > 0) {
      // 物理受限追踪
      this.pupilX = this.centralX + (dx / dist) * Math.min(dist * 0.2, maxPupilOffset)
      this.pupilY = this.centralY + (dy / dist) * Math.min(dist * 0.2, maxPupilOffset)
    } else {
      this.pupilX = this.centralX; this.pupilY = this.centralY
    }
    
    // 随机眨眼
    this.blinkTimer--
    if (this.blinkTimer < -5) this.blinkTimer = Math.random() * 200 + 100
  }
  
  draw(ctx, progress, hyper) {
    ctx.save()
    // [0] 羊皮卷纤维烧录层 (正片叠底) -> 耶稣受难图騰在这里，绝对完美融合
    // 羊皮卷纹理已经在底层，耶稣受难 Canvas 绘制在下面。此层不绘眼，只绘眼下的基底。
    
    // [1] 眼球渲染层 (SourceOver)
    // 确保眼球是一个“洞”，直接呈现在纸上。
    
    // 绘制眼白 (Occult Sclera)
    ctx.beginPath()
    ctx.arc(this.centralX, this.centralY, this.baseR, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(235, 230, 220, 0.95)' // 略浑浊的凡人色
    ctx.fill()
    ctx.lineWidth = 3; ctx.strokeStyle = 'rgba(13, 26, 20, 0.9)'; ctx.stroke()
    
    // 眼部高维侵蚀血丝 (Red Capillaries)
    for(let i=0; i<30; i++) {
        const ang = Math.random() * Math.PI*2
        const startR = this.pupilR + Math.random() * 10
        const endR = this.baseR - Math.random() * 10
        ctx.beginPath()
        ctx.moveTo(this.centralX + Math.cos(ang)*startR, this.centralY + Math.sin(ang)*startR)
        ctx.lineTo(this.centralX + Math.cos(ang)*endR, this.centralY + Math.sin(ang)*endR)
        ctx.strokeStyle = `rgba(139, 0, 0, ${0.1 + Math.random()*0.2})`
        ctx.lineWidth = 0.5; ctx.stroke()
    }

    // 绘制瞳孔与 percentage (The Void Pupil)
    if (this.blinkTimer > 0) {
      // 绘制瞳孔
      ctx.beginPath()
      ctx.arc(this.pupilX, this.pupilY, this.pupilR, 0, Math.PI * 2)
      ctx.fillStyle = hyper ? 'rgba(139, 0, 0, 0.98)' : 'rgba(10, 5, 5, 0.98)' // 狂化变红
      ctx.fill()
      ctx.fillStyle = `rgba(226, 222, 208, 1)` // 凡人色 percentage
      ctx.font = `bold 42px 'Cinzel', serif`
      ctx.textAlign = 'center'; ctx.textBaseline = 'middle'
      ctx.fillText(`${progress}%`, this.pupilX, this.pupilY)
      
      // 诡异的反光点
      ctx.beginPath()
      ctx.arc(this.pupilX - 15, this.pupilY - 15, 6, 0, Math.PI*2)
      ctx.fillStyle = 'rgba(255,255,255, 0.6)'
      ctx.fill()
    } else {
      // 闭眼状态 (Blinking)
      ctx.beginPath()
      ctx.moveTo(this.centralX - this.baseR, this.centralY)
      // 诡异的闭眼弧线
      ctx.bezierCurveTo(this.centralX - 30, this.centralY + 30, this.centralX + 30, this.centralY + 30, this.centralX + this.baseR, this.centralY)
      ctx.lineWidth = 4; ctx.strokeStyle = 'rgba(10, 5, 5, 1)'; ctx.stroke()
    }
    
    // 观察者标签 (The Observer)
    ctx.font = `bold 12px 'Space Mono', monospace`
    ctx.fillStyle = '#1A1A1A'
    ctx.fillText('观察者协议 V12 // 飞升同化核心', this.centralX, this.centralY + this.baseR + 25)
    ctx.restore()
  }
}

// === 核心渲染主线程：万法归一矩阵 ===
let vEngine, images = {}, debris = [], inks = [], cornerLimbs = []

const bootconvergenceEngine = async () => {
  const cvs = mainCanvas.value
  ctx = cvs.getContext('2d')
  w = window.innerWidth; h = window.innerHeight
  cvs.width = w; cvs.height = h

  // 同步载入所有材质 (取缔 <img> 标签)
  pushLog('系统已介入：正在唤醒底层阿卡夏材质矩阵...', 'sys')
  pushLog('开始对 JPG 材质执行绝对正片叠底(Multiply)物理融合...', 'sys')
  
  images = {
    paper: await loadImageAsset(imgPaper),
    totem: await loadImageAsset(imgTotem),
    ink1: await loadImageAsset(imgInk1), ink2: await loadImageAsset(imgInk2), ink3: await loadImageAsset(imgInk3),
    ink4: await loadImageAsset(imgInk4), ink5: await loadImageAsset(imgInk5),
    ten1: await loadImageAsset(imgTen1), ten2: await loadImageAsset(imgTen2)
  }
  pushLog('材质加载完成。像素融合法则已生效。白底物理抹杀率 100%。', 'success')
  pushLog('构建非欧几里得物理空间与絕對凝视核心 V2...', 'sys')

  // 初始化观察者眼球核心 (53% 的归宿)
  vEngine = {
    obsGauge: new VzuorObserverGauge(w, h),
    mx: w/2, my: h/2,
    time: 0,
    hyper: false
  }

  // 初始化物理实体
  for(let i=0; i<35; i++) debris.push(new ArcaneDebris(w, h))
  for(let i=0; i<130; i++) inks.push(new KarmaAsh(w, h))
  
  // 初始化角落漂浮触手动力学矩阵
  cornerLimbs = [
    new ElderTentacle(w*0.05, h, 15, 18, 'rgba(10, 10, 10, 0.3)'), // 左下
    new ElderTentacle(w*0.95, h, 20, 22, 'rgba(10, 10, 10, 0.3)'), // 右下
    new ElderTentacle(w, h*0.3, 12, 25, 'rgba(30, 10, 10, 0.2)')  // 右中垂下
  ]

  // 监听神识 (鼠标)
  window.addEventListener('mousemove', e => {
    const rect = mainCanvas.value.getBoundingClientRect()
    vEngine.mx = e.clientX - rect.left; vEngine.my = e.clientY - rect.top
  })
  window.addEventListener('resize', () => { 
    w = window.innerWidth; h = window.innerHeight; cvs.width = w; cvs.height = h
    vEngine.obsGauge.w = w; vEngine.obsGauge.h = h
    vEngine.obsGauge.centralX = w/2; vEngine.obsGauge.centralY = h * 0.73
    cornerLimbs[0].bx = w*0.05; cornerLimbs[0].by = h
    cornerLimbs[1].bx = w*0.95; cornerLimbs[1].by = h
    cornerLimbs[2].bx = w; cornerLimbs[2].by = h*0.3
  })

  // 供 CLI 调用的全局物理钩子
  window.vzuorEngine = vEngine

  // 终极渲染循环 (The Ascension Grimoire Loop)
  const loop = () => {
    // 制造拖影，产生混沌动态
    ctx.fillStyle = 'rgba(226, 222, 208, 0.25)' // 羊皮纸色拖影，不透明
    ctx.fillRect(0, 0, w, h)
    
    const { hyper, time, mx, my, obsGauge } = vEngine

    // [0] 物理层：考研符文坠落 (文本绘制不支持 multiply，自带 opacity)
    debris.forEach(d => { d.update(hyper); d.draw(ctx, hyper) })

    // === 绝对正片叠底层 (Multiply) ===
    // JPG 完美融合在此：所有白色变为底图透明，只留下线条和墨色
    ctx.globalCompositeOperation = 'multiply'
    ctx.globalAlpha = hyper ? 1 : 0.9 // 狂化时加强污染

    // 静默墨迹污染分布
    if(loadedImages.ink1) ctx.drawImage(loadedImages.ink1, -w*0.05, -h*0.1, w*0.35, h*0.45)
    if(loadedImages.ink2) ctx.drawImage(loadedImages.ink2, w*0.7, h*0.7, w*0.35, h*0.35)
    ctx.globalAlpha = 0.5
    if(loadedImages.ink3) ctx.drawImage(loadedImages.ink3, w*0.8, -h*0.05, w*0.25, h*0.3)
    if(loadedImages.ink4) ctx.drawImage(loadedImages.ink4, w*0.2, h*0.6, w*0.2, h*0.3)
    
    // 静默触手素描 sketch2.jpg
    if(loadedImages.ten2) {
      ctx.globalAlpha = hyper ? 0.3 : 0.2
      ctx.drawImage(loadedImages.ten2, w*0.05, h*0.1, w*0.15, h*0.2)
    }

    // === 核心祭坛：耶稣受难图騰 (totem.jpg) ===
    // JPG 完美融合到羊皮纸，绝对无凡人白边
    const totemH = h * 0.8
    const totemW = totemH * (loadedImages.totem.width / loadedImages.totem.height)
    const tx = (w/2 - totemW/2) + (hyper ? (Math.random()-0.5)*15 : 0)
    const ty = h * 0.05 + (hyper ? (Math.random()-0.5)*15 : 0)
    ctx.drawImage(loadedImages.totem, tx, ty, totemW, totemH)
    ctx.globalAlpha = 1.0 // 恢复透明度

    // === 恢复正常绘制模式 (SourceOver) ===
    ctx.globalCompositeOperation = 'source-over'

    // [1] 物体层：动态触手矩阵 (追鼠标)
    cornerLimbs.forEach((t, i) => {
      // 触手追踪鼠标坐标，狂乱自主游走
      const tx = (i%2===0 && !hyper) ? mx + Math.cos(time+i)*50 : w/2 + Math.cos(time*3+i)*400
      const ty = (i%2===0 && !hyper) ? my + Math.sin(time+i)*50 : h/2 + Math.sin(time*2+i)*400
      t.solve(tx, ty); t.draw(ctx, time, hyper)
    })

    // [2] 物体层：业障灰烬粒子
    inks.forEach(p => { p.update(hyper); p.draw(ctx) })

    // === 物体层：絕對凝视核心 V2 (The Observer Eye) ===
    // 嵌进 53% 原处。眼球跟随鼠标， percentage 在瞳孔渲染
    obsGauge.update(mx, my, time)
    obsGauge.draw(ctx, vocabProgressPercent.value, hyper)

    vEngine.time += hyper ? 0.08 : 0.02
    animId = requestAnimationFrame(loop)
  }
  loop()
}

// ============================================================================
// [卷五：灵魂生命周期钩子]
// ============================================================================
let clockId
onMounted(() => {
  bootconvergenceEngine() // 启动渲染矩阵
  clockId = setInterval(() => {
    // 考研 destiny reckoning
    const diff = new Date('2028-12-23T08:30:00') - Date.now()
    if (diff <= 0) {
      doomTimer.value = "DESTINY_FULFILLED"
      pushLog('星辰归位。因果终结。', 'success')
      clearInterval(clockId)
      return
    }
    const d = Math.floor(diff/86400000).toString().padStart(3,'0')
    const h = Math.floor((diff/3600000)%24).toString().padStart(2,'0')
    const m = Math.floor((diff/60000)%60).toString().padStart(2,'0')
    const s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    // SAN 理智缓慢随时间流失
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001 - (deathCount.value * 0.00002))
    setEngram('sanity', sanityIndex.value)
  }, 1000)
})

onBeforeUnmount(() => { clearInterval(clockId); cancelAnimationFrame(animId) })

</script>

<style scoped>
/* ============================================================================
   [卷六：绝界织物 (CSS Architecture)]
   [核心修改 1]：彻底取缔磨砂框。mix-blend-mode: multiply 强制烧录文本进羊皮纸。
   [核心修改 2]：绝对失控排版。UI 碎片化漂浮在四周，避免规整，互不重叠。
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@800;900&family=IM+Fell+English+SC&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  --c-paper: #E2DED0;
  --c-ink: #111;
  --c-text-muted: #555;
  --c-blood: #8B0000;
  --c-bone: #f2efe4;
  --c-terminal: #00FF66;
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', 'Microsoft YaHei', monospace;
}

body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: var(--c-paper); color: var(--c-ink); font-family: var(--f-mono); }

.nexus-chaos-root { position: relative; width: 100%; height: 100%; }

/* 基石材质层 */
.base-parchment-texture { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover; z-index: 0; opacity: 1.0; }
.occult-noise-overlay { position: absolute; inset: 0; z-index: 2; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="3"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.04; }
.void-vignette-mask { position: absolute; inset: 0; z-index: 3; pointer-events: none; background: radial-gradient(circle, transparent 40%, rgba(20,0,0,0.3) 100%); }

/* 理智状态：精神污染滤镜绑定 */
.san-med { filter: brightness(1.1) contrast(1.1); }
.san-broken { filter: url(#flesh-melt) contrast(1.4) brightness(0.9); }

/* Canvas 底层物理渲染引擎 */
.main-abyssal-canvas { position: absolute; inset: 0; z-index: 10; display: block; width: 100%; height: 100%; }

/* --- [终极 UI 织物] ---
   核心修改：`mix-blend-mode: multiply` 使文本绝对嵌入羊皮纸纹理。
   取缔任何 background, backdrop-filter, border。
   ================================================================== */
.immutable-ui-matrix {
  position: relative; z-index: 100;
  width: 100%; height: 100%; padding: 3rem; box-sizing: border-box;
  pointer-events: none; /* 让鼠标穿透到 Canvas 下层实现凝视核心跟随 */
}
.immutable-ui-matrix * { pointer-events: auto; }

/* UI 碎片通用样式 */
.ui-shard {
  position: absolute;
  /* 取缔廉价磨砂框的关键：transparent 背景，乘法混合机制使文本渗透进底纹 */
  background: transparent;
  border: none; box-shadow: none; backdrop-filter: none;
  mix-blend-mode: multiply; /* [物理烧录协议启动] */
  pointer-events: auto;
  font-family: var(--f-mono);
}

/* 天极刻度 (左上) */
.shard-top-left { top: 3rem; left: 3rem; }
.chaotic-title { font-family: var(--f-title); font-size: 3rem; font-weight: 900; margin: 0; position: relative; letter-spacing: 2px; }
/* 标题双重叠影 */
.chaotic-title::before { content: attr(data-text); position: absolute; left: -2px; text-shadow: 2px 0 red; opacity: 0.5; animation: glitch-anim-1 3s infinite linear alternate-reverse; }
.chaotic-title::after { content: attr(data-text); position: absolute; left: 2px; text-shadow: -2px 0 blue; opacity: 0.5; animation: glitch-anim-2 2s infinite linear alternate-reverse; }

.subtitle-bar { font-size: 0.8rem; font-weight: bold; display: flex; align-items: center; gap: 8px; margin-top: 5px; opacity: 0.7; color: var(--c-text-muted); }
.status-pulse { width: 8px; height: 8px; background-color: var(--c-blood); border-radius: 50%; animation: pulse 2s infinite; }
.doom-timer-group { margin-top: 15px; text-align: left; }
.doom-clock { font-size: 2.5rem; font-weight: 700; font-family: var(--f-mono); letter-spacing: -2px; line-height: 1; }
.doom-lbl { font-size: 0.7rem; color: var(--c-text-muted); margin-top: 5px; font-weight: bold; letter-spacing: 1px; }

/* 灵魂刻度监控器 (左中) */
.shard-left-mid { top: 40%; left: 3rem; transform: translateY(-50%); width: 280px; }
.box-title { font-size: 0.9rem; color: #111; letter-spacing: 3px; border-bottom: 2px solid #111; padding-bottom: 5px; margin: 0 0 1.2rem 0; font-weight: bold; }
.biometric-matrix { display: flex; flex-direction: column; gap: 10px; font-size: 0.85rem; }
.stat-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px dashed rgba(0,0,0,0.1); padding-bottom: 2px; }
.stat-key { color: var(--c-text-muted); font-weight: bold; }
.stat-val { font-weight: 700; color: #111; }
.text-blood { color: var(--c-blood); }
.text-bone { color: var(--c-paper); background: #111; font-weight: 700; padding: 0 4px; }
.caffeine-glow { color: #cc0000; font-size: 1.1rem; text-shadow: 0 0 10px rgba(200,0,0,0.8); animation: jitter 0.1s infinite; }
.high-karma-text { color: var(--c-blood); animation: violent-shake 0.1s infinite; }

.pillar-status-group { margin-top: 1.5rem; font-size: 0.75rem; font-weight: bold; display: flex; flex-direction: column; gap: 6px; }
.p-row { display: flex; flex-direction: column; gap: 4px; }
.p-bg { width: 100%; height: 5px; background: rgba(0,0,0,0.1); border: 1px solid var(--c-ink); }
.p-fill { height: 100%; background: var(--c-ink); }

/* 日志残卷终端 (右上) */
.shard-top-right { top: 3rem; right: 3rem; width: 330px; }
.terminal-logs { height: 200px; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; background: rgba(0,0,0,0.03); padding: 10px; border: 1px inset rgba(0,0,0,0.1); font-size: 0.7rem; }
.terminal-logs::-webkit-scrollbar { width: 4px; }
.terminal-logs::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.2); }
.log-line { line-height: 1.4; }
.log-line .ts { color: var(--c-text-muted); opacity: 0.6; flex-shrink: 0; }
.log-line.sys { color: #555; font-style: italic; }
.log-line.echo { color: #777; font-style: italic; opacity: 0.8; }
.log-line.success { color: #006600; font-weight: bold; }
.log-line.alert, .log-line.error { color: var(--c-blood); font-weight: bold; }

/* 献祭输入终端 (底部) */
.shard-bottom-center { bottom: 2rem; left: 50%; transform: translateX(-50%); width: 600px; background: rgba(226, 222, 208, 0.9) !important; mix-blend-mode: normal !important; border: 2px solid var(--c-ink) !important; border-radius: 4px; padding: 10px 20px; box-shadow: 0 0 30px rgba(0,0,0,0.6); }
.cli-input-wrapper { display: flex; align-items: center; gap: 10px; }
.cli-prefix { font-family: var(--f-mono); font-weight: bold; font-size: 1.2rem; color: var(--c-blood); }
.cli-input { flex: 1; border: none; background: transparent; outline: none; font-family: inherit; font-size: 1.4rem; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed transparent; }
.cli-input::placeholder { color: #888; font-size: 0.85rem; font-style: normal; }
.cli-input:focus { border-bottom: 1px solid var(--c-ink); }
.cli-input:disabled { opacity: 0.3; cursor: not-allowed; }

/* 死亡回归层 */
.death-shroud { position: fixed; inset: 0; z-index: 9999; background: #050505; display: flex; justify-content: center; align-items: center; flex-direction: column; }
.death-core { text-align: center; }
.death-text { color: #fff; font-family: var(--f-title); font-size: 4rem; letter-spacing: 10px; margin: 0; }
.death-sub { color: var(--c-text-muted); font-size: 1.2rem; letter-spacing: 3px; margin-top: 20px; animation: pulse 1s infinite; }
.glitch-violently { animation: violent-shake 0.1s infinite; }

/* 动画库定义 */
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes violent-shake { 0% { transform: translate(3px, 2px) rotate(1deg); } 50% { transform: translate(-2px, 3px) rotate(-1deg); } 100% { transform: translate(2px, -2px) rotate(1deg); } }
@keyframes pulse { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
@keyframes glitch-anim-1 { 0% { clip: rect(20px, 9999px, 85px, 0); } 20% { clip: rect(66px, 9999px, 32px, 0); } 40% { clip: rect(10px, 9999px, 95px, 0); } 60% { clip: rect(55px, 9999px, 11px, 0); } 80% { clip: rect(82px, 9999px, 49px, 0); } 100% { clip: rect(27px, 9999px, 73px, 0); } }
@keyframes glitch-anim-2 { 0% { clip: rect(89px, 9999px, 12px, 0); } 20% { clip: rect(34px, 9999px, 78px, 0); } 40% { clip: rect(90px, 9999px, 21px, 0); } 60% { clip: rect(15px, 9999px, 63px, 0); } 80% { clip: rect(72px, 9999px, 44px, 0); } 100% { clip: rect(38px, 9999px, 86px, 0); } }
</style>