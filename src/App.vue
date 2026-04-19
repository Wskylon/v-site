/* =========================================================================================================
   [阿卡姆终极飞升魔典 V11.0 // 万法归一 THE ULTIMATE CONVERGENCE]
   [宿主：武少康 // 目标：22408 科软飞升 // 精神状态：临界]
   [系统备注：已启动 Canvas 绝对正片叠底引擎。DOM 图像渲染已被彻底取缔，白底物理抹杀率 100%。]
   [代码量预警：包含庞大阿卡夏法典库、流体动力学引擎、反向运动学(IK)触手矩阵，总体积已膨胀至极点。]
   ========================================================================================================= */

<template>
  <div id="vzuor-ascension" class="nexus-chaos-root" :class="sanityLevelClass">
    
    <svg width="0" height="0" class="occult-filters">
      <defs>
        <filter id="abyssal-glitch">
          <feTurbulence type="fractalNoise" baseFrequency="0.04 0.8" numOctaves="4" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="8" xChannelSelector="R" yChannelSelector="G" />
          <feColorMatrix type="matrix" values="1 0 0 0 0  0 0.2 0 0 0  0 0.2 0 0 0  0 0 0 1 0" />
        </filter>
        <filter id="flesh-melt">
          <feGaussianBlur in="SourceGraphic" stdDeviation="2" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="goo" />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
      </defs>
    </svg>
    <div class="static-noise-overlay"></div>
    <div class="void-vignette"></div>

    <canvas ref="mainCanvas" class="abyssal-physics-canvas"></canvas>

    <div class="shattered-ui-layer">
      
      <div class="shard shard-top-left">
        <h1 class="chaotic-title" data-text="V'ZUOR KHAA-SH'AN">V'ZUOR KHAA-SH'AN</h1>
        <div class="subtitle-bar">
          <span class="pulse-dot"></span> [协议 22408] 观测者枢纽已接管
        </div>
        <div class="doom-clock">
          <span class="clock-val">{{ doomTimer }}</span>
          <span class="clock-lbl">距星辰归位之时 (2028-12)</span>
        </div>
      </div>

      <div class="shard shard-bottom-left">
        <h2 class="box-title">灵魂与肉身刻度</h2>
        <div class="stat-grid">
          <div class="stat-row">
            <span class="stat-key">理智残留 (SAN):</span>
            <span class="stat-val text-blood">{{ sanityIndex.toFixed(4) }}%</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">业障权重 (KARMA):</span>
            <span class="stat-val" :class="{'glow-alert': karmaWeight > 50}">{{ karmaWeight.toFixed(2) }}</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">肉身炼金循环:</span>
            <span class="stat-val text-bone">3-ON / 2-OFF</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">灵药摄入剂量:</span>
            <span class="stat-val" :class="{'hyper-glow': isHyper}">{{ caffeineLvl }} MG</span>
          </div>
        </div>
        
        <div class="pillar-status">
          <div class="p-row">
            <span class="p-name">Ⅰ 血肉 (C语言)</span>
            <div class="p-bar-bg"><div class="p-bar-fill" style="width: 45%;"></div></div>
          </div>
          <div class="p-row">
            <span class="p-name">Ⅱ 骸骨 (数据结构)</span>
            <div class="p-bar-bg"><div class="p-bar-fill" style="width: 25%;"></div></div>
          </div>
          <div class="p-row">
            <span class="p-name">Ⅲ 虚空 (高阶数学)</span>
            <div class="p-bar-bg"><div class="p-bar-fill" style="width: 10%;"></div></div>
          </div>
        </div>
      </div>

      <div class="shard shard-top-right">
        <h2 class="box-title">阿卡夏记录 (交互日志)</h2>
        <div class="terminal-logs" ref="logContainer">
          <div v-for="(log, idx) in logs" :key="idx" class="log-line" :class="log.type">
            <span class="ts">[{{ log.time }}]</span>
            <span class="msg" v-html="log.msg"></span>
          </div>
        </div>
      </div>

      <div class="shard shard-bottom-right">
        <div class="cli-wrapper">
          <span class="prompt">root@vzuor:~#</span>
          <input 
            v-model="cmdInput" 
            @keyup.enter="performRitual"
            class="cli-input" 
            placeholder="献祭进度 (vocab 50) 或注入灵药 (dose 400)..."
            spellcheck="false" autocomplete="off"
            :disabled="isDead"
          />
        </div>
      </div>

    </div>

    <div class="death-shroud" v-if="isDead">
      <div class="death-core">
        <h1 class="death-words glitch">灵魂剥离 · 因果重置</h1>
        <p class="death-sub">业障已刻印，世界线正在重构...</p>
      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, computed, nextTick } from 'vue'

// ============================================================================
// [卷一：真名材质静态导入]
// Vite 的绝对真理。通过 import 确保打包后路径 100% 正确，不再有 404 碎图。
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
// [卷二：阿卡夏状态机与持久化]
// ============================================================================
const getStore = (key, def) => Number(localStorage.getItem(`v11_${key}`)) || def
const setStore = (key, val) => localStorage.setItem(`v11_${key}`, val)

const vocabSiphoned = ref(getStore('vocab', 2150))
const karmaWeight = ref(getStore('karma', 23.7))
const sanityIndex = ref(getStore('sanity', 99.9998))
const caffeineLvl = ref(200)

const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref('')
const isHyper = ref(false)
const isDead = ref(false)
const logs = reactive([])
const logContainer = ref(null)

const vocabPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

const sanityLevelClass = computed(() => {
  if (sanityIndex.value > 80) return 'san-safe'
  if (sanityIndex.value > 40) return 'san-warn'
  return 'san-danger'
})

const getTs = () => {
  const d = new Date(); 
  return `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}`
}

const pushLog = (msg, type = 'info') => {
  logs.push({ time: getTs(), msg, type })
  if (logs.length > 80) logs.shift()
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

// ============================================================================
// [卷三：庞大的考研法典库 (坠落碎片的知识源泉)]
// 满足深度与代码量需求，将知识点异化为邪神低语
// ============================================================================
const arcaneLoreDatabase = [
  // C 语言血肉篇
  "C: 指针即虚空坐标，解引用即凝视深渊", "C: malloc 借用亡者血肉，free 归还因果", "C: 数组退化为指针，犹如理智的溃散",
  "C: 悬空指针游荡在栈帧的墓地", "C: 结构体是对血肉进行强行拼凑的缝合怪", "C: 宏定义是预处理器的远古低语",
  "C: void* 是包容一切形态的无形之物", "C: 段错误 (SegFault) 是神明降下的天罚",
  // 数据结构骸骨篇
  "DS: 链表是首尾相连的死者之脉", "DS: 栈是先进后出的窒息囚笼", "DS: 队列是走向祭坛的无尽长龙",
  "DS: 二叉树不断分裂，长出非欧几里得的枝丫", "DS: 图论遍历 (DFS/BFS) 是在迷宫中寻找出口的疯狂",
  "DS: 哈希表将灵魂映射到虚无的槽位", "DS: 快速排序通过不断撕裂数组来重建秩序", "DS: 动态规划是窥探未来所有时间线的邪术",
  // 高阶数学虚空篇
  "MATH: 极限 (Limit) 永远逼近，却永远无法触碰真理", "MATH: 导数 (Derivative) 瞬间撕裂了平滑的现实曲线",
  "MATH: 洛必达法则 (L'Hôpital) 在 0/0 的混沌中寻找出口", "MATH: 多重积分吞噬了整个维度的体积",
  "MATH: 泰勒展开 (Taylor Series) 将有限的肉身拆解为无限的碎片", "MATH: 微分方程描述了灵魂衰变的必然规律",
  "MATH: 矩阵乘法 (Matrix) 是空间维度的暴力扭曲", "MATH: 特征值与特征向量是物体的真实骨架"
]

// ============================================================================
// [卷四：终极 2D 神秘学物理引擎 (The Ultimate Chaos Engine)]
// 负责完美融合所有 JPG (取缔 CSS mix-blend-mode)、触手动力学、粒子与坠落系统
// ============================================================================
const mainCanvas = ref(null)
let ctx, w, h, animId, time = 0
const loadedImages = {} // 存储 Image 对象

// 加载图片工厂
const loadTexture = (src) => {
  return new Promise((resolve) => {
    const img = new Image()
    img.src = src
    img.onload = () => resolve(img)
  })
}

// === 物理类：坠落的考研知识符文 (Falling Debris) ===
class FallingRune {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = Math.random() * this.w
    this.y = - (Math.random() * 500 + 50) // 从天上很高的地方开始掉
    this.vx = (Math.random() - 0.5) * 1.5
    this.vy = Math.random() * 1.5 + 0.5 // 坠落速度
    this.angle = Math.random() * Math.PI * 2
    this.va = (Math.random() - 0.5) * 0.05 // 旋转速度
    this.text = arcaneLoreDatabase[Math.floor(Math.random() * arcaneLoreDatabase.length)]
    this.size = Math.random() * 8 + 12 // 字体大小
    this.opacity = Math.random() * 0.5 + 0.2
  }
  update(isHyper) {
    this.x += this.vx * (isHyper ? 3 : 1)
    this.y += this.vy * (isHyper ? 4 : 1)
    this.angle += this.va * (isHyper ? 3 : 1)
    // 掉出屏幕底部则重置
    if (this.y > this.h + 100) this.reset()
  }
  draw(ctx) {
    ctx.save()
    ctx.translate(this.x, this.y)
    ctx.rotate(this.angle)
    ctx.globalCompositeOperation = 'multiply' // 正片叠底融入羊皮纸
    ctx.font = `${this.size}px 'Space Mono', 'Microsoft YaHei', monospace`
    ctx.fillStyle = `rgba(30, 20, 20, ${this.opacity})`
    ctx.fillText(this.text, 0, 0)
    ctx.restore()
  }
}

// === 物理类：漂浮升腾的虚空墨水粒子 (Floating Ink) ===
class FloatingInk {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = (this.w / 2) + (Math.random() - 0.5) * 600 // 集中在中央祭坛附近生成
    this.y = this.h + Math.random() * 200
    this.vx = (Math.random() - 0.5) * 0.8
    this.vy = - (Math.random() * 2 + 0.5) // 向上漂浮
    this.life = Math.random() * 300 + 100; this.maxLife = this.life
    this.r = Math.random() * 4 + 1
    this.isBlood = Math.random() > 0.8 // 20%的概率是血红色的业障粒子
  }
  update(isHyper) {
    this.x += this.vx * (isHyper ? 2 : 1)
    this.y += this.vy * (isHyper ? 2 : 1)
    this.life--
    if (this.life < 0 || this.y < -50) this.reset()
  }
  draw(ctx) {
    const op = (this.life / this.maxLife) * 0.7
    ctx.globalCompositeOperation = 'multiply'
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2)
    ctx.fillStyle = this.isBlood ? `rgba(139, 0, 0, ${op})` : `rgba(20, 20, 20, ${op})`
    ctx.fill()
    ctx.globalCompositeOperation = 'source-over'
  }
}

// === 物理类：反向运动学触手 (IK Tentacles) ===
class IKTentacle {
  constructor(bx, by, numSegments, segmentLength, color) {
    this.bx = bx; this.by = by
    this.num = numSegments; this.len = segmentLength
    this.joints = Array(numSegments).fill({ x: bx, y: by })
    this.color = color
    this.noise = Math.random() * 1000
  }
  // FABRIK 算法变体
  reach(tx, ty) {
    this.joints[this.num - 1] = { x: tx, y: ty }
    for (let i = this.num - 2; i >= 0; i--) {
      const dx = this.joints[i].x - this.joints[i + 1].x
      const dy = this.joints[i].y - this.joints[i + 1].y
      const dist = Math.sqrt(dx * dx + dy * dy) || 1
      this.joints[i] = { x: this.joints[i + 1].x + dx * (this.len / dist), y: this.joints[i + 1].y + dy * (this.len / dist) }
    }
    this.joints[0] = { x: this.bx, y: this.by }
    for (let i = 1; i < this.num; i++) {
      const dx = this.joints[i].x - this.joints[i - 1].x
      const dy = this.joints[i].y - this.joints[i - 1].y
      const dist = Math.sqrt(dx * dx + dy * dy) || 1
      this.joints[i] = { x: this.joints[i - 1].x + dx * (this.len / dist), y: this.joints[i - 1].y + dy * (this.len / dist) }
    }
  }
  draw(ctx, time, isHyper) {
    ctx.beginPath()
    ctx.moveTo(this.joints[0].x, this.joints[0].y)
    for (let i = 1; i < this.num; i++) {
      // 加入高频蠕动噪点
      const wob = Math.sin(time * (isHyper ? 6 : 2) + i * 0.3 + this.noise) * (isHyper ? 8 : 3)
      ctx.lineTo(this.joints[i].x + wob, this.joints[i].y + wob)
    }
    ctx.strokeStyle = this.color
    ctx.lineWidth = isHyper ? 4 : 2
    ctx.lineCap = 'round'
    ctx.stroke()
  }
}

// --- 核心渲染主线程 ---
const bootPhysicsEngine = async () => {
  const cvs = mainCanvas.value
  ctx = cvs.getContext('2d')
  w = window.innerWidth; h = window.innerHeight
  cvs.width = w; cvs.height = h

  // 同步加载所有图片到内存 (确保万无一失)
  pushLog('连接底层阿卡夏资源库，正在加载真名图腾...', 'sys')
  loadedImages.paper = await loadTexture(imgPaper)
  loadedImages.totem = await loadTexture(imgTotem)
  loadedImages.ink1 = await loadTexture(imgInk1)
  loadedImages.ink2 = await loadTexture(imgInk2)
  loadedImages.ink3 = await loadTexture(imgInk3)
  loadedImages.ink4 = await loadTexture(imgInk4)
  loadedImages.ink5 = await loadTexture(imgInk5)
  loadedImages.ten1 = await loadTexture(imgTen1)
  loadedImages.ten2 = await loadTexture(imgTen2)
  pushLog('素材解析完成。Canvas 物理渲染矩阵上线。白底死刑已启动。', 'success')

  // 初始化物理实体
  const fallingDebris = Array.from({length: 35}, () => new FallingRune(w, h))
  const floatingInks = Array.from({length: 120}, () => new FloatingInk(w, h))
  const tentacles = [
    new IKTentacle(w*0.3, h, 20, 20, 'rgba(15, 15, 15, 0.4)'),
    new IKTentacle(w*0.7, h, 25, 18, 'rgba(15, 15, 15, 0.5)'),
    new IKTentacle(w*0.5, -100, 30, 25, 'rgba(30, 5, 5, 0.3)') // 从天上下垂的血肉触手
  ]

  let mx = w/2, my = h/2
  window.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY })
  window.addEventListener('resize', () => { 
    w = window.innerWidth; h = window.innerHeight; cvs.width = w; cvs.height = h 
    tentacles[0].bx = w*0.3; tentacles[0].by = h
    tentacles[1].bx = w*0.7; tentacles[1].by = h
    tentacles[2].bx = w*0.5; tentacles[2].by = -100
  })

  // 绑定狂化状态到 window 供控制台调用
  window.chaosEngine = { hyper: false }

  // THE RENDER LOOP
  const loop = () => {
    const hyper = window.chaosEngine.hyper

    // 1. [基石层] 绘制羊皮纸底色
    ctx.globalCompositeOperation = 'source-over'
    ctx.drawImage(loadedImages.paper, 0, 0, w, h)

    // 2. [绝对正片叠底层] 这是解决白底的关键！
    // 开启 Multiply 模式后，JPG 中的白色(255,255,255)与底图相乘结果不变(透明)，黑色留下。
    ctx.globalCompositeOperation = 'multiply'

    // 绘制四周的墨迹与素描 (打乱排版，肆意放置)
    ctx.globalAlpha = 0.85
    ctx.drawImage(loadedImages.ink1, -w*0.05, -h*0.1, w*0.4, h*0.5)
    ctx.drawImage(loadedImages.ink2, w*0.65, h*0.6, w*0.4, h*0.4)
    
    ctx.globalAlpha = 0.6
    ctx.drawImage(loadedImages.ink3, w*0.75, -h*0.05, w*0.3, h*0.4)
    ctx.drawImage(loadedImages.ink4, -w*0.1, h*0.7, w*0.3, h*0.3)
    
    ctx.globalAlpha = 0.3
    ctx.drawImage(loadedImages.ten1, w*0.05, h*0.2, w*0.2, h*0.3)
    ctx.drawImage(loadedImages.ten2, w*0.7, h*0.2, w*0.25, h*0.35)

    // 3. [中心图腾祭坛]
    ctx.globalAlpha = 1.0
    const totemH = h * 0.75
    const totemW = totemH * (loadedImages.totem.width / loadedImages.totem.height)
    const tx = (w/2 - totemW/2) + (hyper ? (Math.random()-0.5)*10 : 0)
    const ty = h * 0.05 + (hyper ? (Math.random()-0.5)*10 : 0)
    ctx.drawImage(loadedImages.totem, tx, ty, totemW, totemH)

    // 恢复正常模式绘制动态物理对象 (触手、文字等自带 rgba，无需叠底)
    ctx.globalCompositeOperation = 'source-over'

    // 4. [进度黑水池 (53%的归宿)]
    // 将 53% 缩小并封印在正下方耶稣手部下方的位置
    const poolW = 160, poolH = 160
    const poolX = w/2 - poolW/2, poolY = h * 0.78
    
    // 玻璃外壳
    ctx.beginPath()
    ctx.arc(w/2, poolY + poolH/2, poolW/2, 0, Math.PI*2)
    ctx.fillStyle = 'rgba(226, 222, 208, 0.6)'
    ctx.fill()
    ctx.lineWidth = 2; ctx.strokeStyle = 'rgba(20,20,20,0.8)'; ctx.stroke()

    // 黑水填充 (根据百分比计算高度裁剪)
    const fillH = poolH * (vocabPercent.value / 100)
    ctx.save()
    ctx.beginPath(); ctx.arc(w/2, poolY + poolH/2, poolW/2, 0, Math.PI*2); ctx.clip() // 限制在圆形内
    
    // 波动的水面
    ctx.beginPath()
    ctx.moveTo(poolX, poolY + poolH - fillH)
    for (let i = 0; i <= poolW; i += 10) {
      const wave = Math.sin(time * 2 + i * 0.05) * (hyper ? 15 : 5)
      ctx.lineTo(poolX + i, poolY + poolH - fillH + wave)
    }
    ctx.lineTo(poolX + poolW, poolY + poolH); ctx.lineTo(poolX, poolY + poolH)
    ctx.fillStyle = 'rgba(15, 18, 15, 0.95)'; ctx.fill()
    ctx.restore()

    // 53% 数字嵌在水池中
    ctx.font = `bold 48px 'Cinzel', serif`
    ctx.fillStyle = vocabPercent.value > 50 ? 'rgba(220,220,220,0.9)' : 'rgba(139,0,0,0.9)' // 水位没过一半字变白
    ctx.textAlign = 'center'; ctx.textBaseline = 'middle'
    ctx.fillText(`${vocabPercent.value}%`, w/2, poolY + poolH/2)
    ctx.font = `14px 'Space Mono', monospace`
    ctx.fillStyle = '#333'
    ctx.fillText(`${vocabSiphoned.value}/4000 灵魂`, w/2, poolY + poolH + 20)

    // 5. [物理模拟更新与渲染]
    fallingDebris.forEach(d => { d.update(hyper); d.draw(ctx) })
    floatingInks.forEach(p => { p.update(hyper, w/2, h/2); p.draw(ctx) })

    tentacles.forEach((t, i) => {
      // 触手追踪鼠标，狂乱时乱舞
      const targetX = hyper ? w/2 + Math.cos(time*3+i)*400 : mx + Math.cos(time+i)*50
      const targetY = hyper ? h/2 + Math.sin(time*2+i)*400 : my + Math.sin(time+i)*50
      t.reach(targetX, targetY)
      t.color = hyper ? 'rgba(139, 0, 0, 0.7)' : t.color // 狂化变红
      t.draw(ctx, time, hyper)
    })

    time += hyper ? 0.08 : 0.02
    animId = requestAnimationFrame(loop)
  }
  loop()
}

// ============================================================================
// [卷五：生命周期与终端交互]
// ============================================================================
let clockId
onMounted(() => {
  pushLog('V11.0 终极魔典引导序列启动。', 'sys')
  pushLog('正在构建非欧几里得物理引擎...', 'sys')
  
  // 必须先挂载 Canvas 再初始化引擎
  nextTick(() => { bootPhysicsEngine() })
  
  clockId = setInterval(() => {
    const diff = new Date('2028-12-23T08:30:00') - Date.now()
    const d = Math.floor(diff/86400000).toString().padStart(3,'0')
    const h = Math.floor((diff/3600000)%24).toString().padStart(2,'0')
    const m = Math.floor((diff/60000)%60).toString().padStart(2,'0')
    const s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    // 理智随时间缓慢流失
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001)
    setStore('sanity', sanityIndex.value)
  }, 1000)
})

onBeforeUnmount(() => { clearInterval(clockId); cancelAnimationFrame(animId) })

// 处理献祭交互
const performRitual = () => {
  const raw = cmdInput.value.trim().toLowerCase()
  if (!raw) return
  pushLog(`> ${raw}`, 'echo')
  const args = raw.split(' ')

  if (args[0] === 'vocab') {
    const amt = parseInt(args[1])
    if (amt > 0) {
      vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
      setStore('vocab', vocabSiphoned.value)
      karmaWeight.value = Math.max(0, karmaWeight.value - amt * 0.05)
      setStore('karma', karmaWeight.value)
      sanityIndex.value = Math.min(100, sanityIndex.value + 0.2)
      pushLog(`吞噬灵魂。中央祭坛水位上升，当前同化率: ${vocabPercent.value}%`, 'success')
    } else {
      pushLog('献祭失败：虚无的供品无法取悦深渊。', 'error')
    }
  } 
  else if (args[0] === 'dose') {
    const dose = parseInt(args[1])
    if (dose > 0) {
      caffeineLvl.value = dose
      if (dose >= 400) {
        isHyper.value = true; if(window.chaosEngine) window.chaosEngine.hyper = true
        pushLog('警告：灵药剂量致死！物理法则正在崩塌！', 'error')
        setTimeout(() => { 
          isHyper.value = false; if(window.chaosEngine) window.chaosEngine.hyper = false; caffeineLvl.value = 200
          pushLog('灵药反噬消退。机体恢复 3-ON 循环。', 'sys')
        }, 7000)
      } else {
        pushLog(`注入灵药。当前神经元催化剂浓度：${dose}MG。`, 'info')
      }
    }
  }
  else if (args[0] === 'vzuor') {
    pushLog('不可名状之物被直视。理智值出现灾难性断崖。', 'error')
    sanityIndex.value -= 20.0
    setStore('sanity', sanityIndex.value)
  }
  else if (args[0] === 'die') {
    isDead.value = true
    pushLog('执行因果律切断。世界线正在重塑...', 'alert')
    setTimeout(() => {
      isDead.value = false
      sanityIndex.value = 100; karmaWeight.value += 15.0
      setStore('sanity', 100); setStore('karma', karmaWeight.value)
      pushLog('死亡回归完成。带着新的业障重新上路。', 'sys')
    }, 4000)
  }
  else {
    karmaWeight.value += 0.8; setStore('karma', karmaWeight.value)
    pushLog('未知的狂乱低语。业障加深。', 'error')
  }
  cmdInput.value = ''
}
</script>

<style scoped>
/* ============================================================================
   [卷六：绝界织物 (CSS Architecture)]
   核心思路：放弃 Grid 强对齐，采用绝对定位让 UI 碎片分散在屏幕四周
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  --c-paper: #E2DED0;
  --c-ink: #111;
  --c-blood: #8B0000;
  --c-muted: #555;
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', 'Microsoft YaHei', monospace;
}

body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: var(--c-paper); color: var(--c-ink); font-family: var(--f-mono); }
.nexus-chaos-root { position: relative; width: 100vw; height: 100vh; }

/* 滤镜与杂色 */
.occult-filters { position: absolute; pointer-events: none; }
.static-noise-overlay { position: absolute; inset: 0; z-index: 2; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.85" numOctaves="3"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.05; }
.void-vignette { position: absolute; inset: 0; z-index: 3; pointer-events: none; background: radial-gradient(circle, transparent 40%, rgba(20,0,0,0.3) 100%); }

/* 状态滤镜绑定 */
.san-warn { filter: contrast(1.1) sepia(0.2); }
.san-danger { filter: url(#flesh-melt) contrast(1.2); }

/* Canvas 底层 */
.abyssal-physics-canvas { position: absolute; inset: 0; z-index: 10; display: block; width: 100%; height: 100%; }

/* --- 碎片化 UI 层 --- */
.shattered-ui-layer { position: absolute; inset: 0; z-index: 20; pointer-events: none; }
.shard { position: absolute; pointer-events: auto; background: rgba(226, 222, 208, 0.7); backdrop-filter: blur(5px); padding: 1.5rem; border: 1px solid rgba(0,0,0,0.2); box-shadow: 0 0 20px rgba(0,0,0,0.1); }

/* 左上：标题 */
.shard-top-left { top: 2rem; left: 2rem; background: transparent; border: none; box-shadow: none; padding: 0; }
.chaotic-title { font-family: var(--f-title); font-size: 3.5rem; font-weight: 900; margin: 0; letter-spacing: 2px; text-shadow: 2px 2px 5px rgba(0,0,0,0.3); }
.subtitle-bar { font-size: 0.8rem; font-weight: bold; display: flex; align-items: center; gap: 8px; margin-top: 5px; color: var(--c-muted); }
.pulse-dot { width: 8px; height: 8px; background: var(--c-blood); border-radius: 50%; animation: pulse 2s infinite; }
.doom-clock { margin-top: 15px; display: flex; flex-direction: column; }
.clock-val { font-size: 2.2rem; font-weight: bold; letter-spacing: -2px; }
.clock-lbl { font-size: 0.75rem; color: var(--c-muted); font-weight: bold; }

/* 左下：刻度监控 */
.shard-bottom-left { bottom: 2rem; left: 2rem; width: 320px; border-radius: 2px; }
.box-title { font-size: 1.1rem; border-bottom: 2px solid var(--c-ink); padding-bottom: 5px; margin: 0 0 15px 0; font-weight: bold; }
.stat-grid { display: flex; flex-direction: column; gap: 10px; font-size: 0.9rem; font-weight: bold; }
.stat-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px dashed rgba(0,0,0,0.1); padding-bottom: 3px; }
.text-blood { color: var(--c-blood); }
.glow-alert { color: var(--c-blood); animation: jitter 0.2s infinite; }
.hyper-glow { color: red; text-shadow: 0 0 10px red; font-size: 1.2rem; animation: jitter 0.1s infinite; }

.pillar-status { margin-top: 1.5rem; font-size: 0.8rem; font-weight: bold; display: flex; flex-direction: column; gap: 8px; }
.p-row { display: flex; flex-direction: column; gap: 4px; }
.p-bar-bg { width: 100%; height: 6px; background: rgba(0,0,0,0.1); border: 1px solid var(--c-ink); }
.p-bar-fill { height: 100%; background: var(--c-ink); }

/* 右上：日志终端 */
.shard-top-right { top: 2rem; right: 2rem; width: 350px; }
.terminal-logs { height: 220px; overflow-y: auto; font-size: 0.75rem; display: flex; flex-direction: column; gap: 6px; background: rgba(0,0,0,0.03); padding: 10px; border: 1px inset rgba(0,0,0,0.1); }
.terminal-logs::-webkit-scrollbar { width: 4px; }
.terminal-logs::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.2); }
.log-line { line-height: 1.4; }
.log-line .ts { color: var(--c-muted); margin-right: 5px; }
.log-line.sys { color: #555; font-style: italic; }
.log-line.success { color: #006600; font-weight: bold; }
.log-line.error, .log-line.alert { color: var(--c-blood); font-weight: bold; }

/* 右下：献祭输入 */
.shard-bottom-right { bottom: 2rem; right: 2rem; width: 450px; background: rgba(226, 222, 208, 0.85); border: 2px solid var(--c-ink); border-radius: 4px; }
.cli-wrapper { display: flex; align-items: center; gap: 10px; }
.prompt { font-weight: bold; font-size: 1.2rem; color: #006600; }
.cli-input { flex: 1; border: none; background: transparent; outline: none; font-size: 1.4rem; font-family: inherit; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed transparent; transition: 0.3s; }
.cli-input:focus { border-bottom: 1px solid var(--c-ink); }
.cli-input::placeholder { color: #888; font-size: 0.85rem; font-style: normal; }
.cli-input:disabled { opacity: 0.3; cursor: not-allowed; }

/* 死亡回归遮罩 */
.death-shroud { position: fixed; inset: 0; z-index: 9999; background: #050505; display: flex; justify-content: center; align-items: center; }
.death-core { text-align: center; }
.death-words { color: #fff; font-family: var(--f-title); font-size: 4rem; letter-spacing: 5px; margin: 0; }
.death-sub { color: var(--c-muted); font-size: 1.2rem; letter-spacing: 3px; margin-top: 20px; animation: pulse 1s infinite; }
.glitch { animation: violent-shake 0.1s infinite; }

/* 动画库 */
@keyframes pulse { 0% { opacity: 0.3; } 50% { opacity: 1; } 100% { opacity: 0.3; } }
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes violent-shake { 0% { transform: translate(3px, 2px) rotate(1deg); } 50% { transform: translate(-2px, 3px) rotate(-1deg); } 100% { transform: translate(2px, -2px) rotate(1deg); } }
</style>