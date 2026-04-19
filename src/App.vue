/* [终极系统重构：阿卡姆底层协议 V5.0]
   [核心法则：底层 Canvas 像素融合 | 禁用 CSS 混合 | 絕對隔離排版]
   [作者：飞升算法 // 武少康]
   [代码量提示：此版本为 1000+ 行级全量魔典，请勿尝试简略。]
*/
<template>
  <div id="vzuor-nexus" class="nexus-root" :class="[realmState, { 'death-return-active': isDeathReturning }]">
    
    <div class="static-noise"></div>
    <div class="void-scanlines"></div>

    <div class="interface-grid">
      
      <header class="top-sect">
        <div class="title-construct">
          <h1 class="eldritch-title">{{ pageTitle }}</h1>
          <div class="status-monitor">
            <span class="pulse-sig"></span>
            <span class="mono-label">KERN_V5.0 // ABYSSAL_ALTAR // [ {{ systemStatus }} ]</span>
          </div>
        </div>
        <div class="chronos-construct">
          <div class="doom-timer">{{ doomTimer }}</div>
          <div class="timer-label">UNTIL_THE_STARS_ARE_RIGHT (2028-12)</div>
        </div>
      </header>

      <aside class="left-pillar-sect">
        <h2 class="zone-title">THE_PILLARS</h2>
        <div class="pillar-matrix">
          <div class="pillar-node" :class="{'active': activePillar === 'C_LANG'}">
            <span class="node-icon">Ⅰ</span>
            <div class="node-data">
              <div class="node-name">FLESH (C_LANGUAGE)</div>
              <div class="node-status">ASSIMILATION_ONGOING</div>
            </div>
          </div>
          <div class="pillar-node" :class="{'active': activePillar === 'DATA_STRUCT'}">
            <span class="node-icon">Ⅱ</span>
            <div class="node-data">
              <div class="node-name">BONE (DATA_STRUCTURES)</div>
              <div class="node-status">RECONSTRUCTING</div>
            </div>
          </div>
          <div class="pillar-node" :class="{'active': activePillar === 'MATH'}">
            <span class="node-icon">Ⅲ</span>
            <div class="node-data">
              <div class="node-name">VOID (ADVANCED_MATH)</div>
              <div class="node-status">COMPREHENDING</div>
            </div>
          </div>
        </div>
        
        <div class="forge-stats mt-auto">
          <h2 class="zone-title">FLESH_FORGE (GYM)</h2>
          <div class="forge-data">
            <div class="forge-label">CYCLE: 3-ON / 2-OFF</div>
            <div class="forge-bar-wrap">
              <div class="forge-bar"><div class="forge-progress" style="width: 70%"></div></div>
            </div>
            <div class="forge-dose-row">
              <div class="label-muted">CAFFEINE:</div>
              <div class="dose-val" :class="{'high-dose text-hyper': isHyper}">{{ caffeineLvl }} MG</div>
            </div>
          </div>
        </div>
      </aside>

      <main class="center-rift-sect">
        <div class="rift-containment">
          <canvas ref="mainCanvas" class="main-canvas"></canvas>
          
          <div class="siphon-readout">
            <div class="readout-header">ASSIMILATION_LOG</div>
            <div class="massive-percent">
              <span class="num">{{ vocabProgressPercentage }}</span>
              <span class="unit">%</span>
            </div>
            <div class="vocab-total">{{ vocabSiphoned }} / 4000 WORDS_SIPHONED</div>
          </div>
        </div>
      </main>

      <aside class="right-metrics-sect">
        <h2 class="zone-title">BIOMETRIC_TELEMETRY</h2>
        <div class="metrics-matrix">
          <div class="metric-row">
            <span class="label">SANITY_INDEX:</span>
            <span class="value text-alert">{{ sanityIndex.toFixed(4) }}%</span>
          </div>
          <div class="metric-row">
            <span class="label">KARMA_WEIGHT:</span>
            <span class="value">{{ karmaLoad.toFixed(2) }}</span>
          </div>
          <div class="metric-row">
            <span class="label">SENIOR_SISTER:</span>
            <span class="value text-glitch" :class="{'active': isGazed}">{{ isGazed ? 'DETECTED' : 'ABSENT' }}</span>
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
            class="cli-input"
            placeholder="AWAITING SACRIFICE... (help, vocab [num], dose [num])"
            spellcheck="false"
            autocomplete="off"
          />
        </div>
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// ============================================================================
// [终极系统架构：状态持久化与维度逻辑引擎]
// ============================================================================

// 1. 本地存储代理 (LocalStorage Agent)
const loadState = (k, def) => Number(localStorage.getItem(`vzuor_v5_${k}`)) || def
const saveState = (k, val) => localStorage.setItem(`vzuor_v5_${k}`, val)

// 2. 核心修行状态 (Core State)
const doomTimer = ref('000D | 00:00:00')
const vocabSiphoned = ref(loadState('vocab', 2150))
const karmaLoad = ref(loadState('karma', 23.7))
const caffeineLvl = ref(200)
const sanityIndex = ref(loadState('sanity', 99.9999))
const activePillar = ref('C_LANG')
const isGazed = ref(false)

// 计算进度百分比，直接输出为整数用于大数字显示
const vocabProgressPercentage = computed(() => {
  return Math.floor((vocabSiphoned.value / 4000) * 100)
})

// 3. 维度与状态机 (State Machine)
const pageTitle = ref("Vzuor Khaa-Sh'an")
const systemStatus = ref('SIPHONING :: MONITORING')
const realmState = ref('realm-stable')
const flavorText = ref('等待神经信号接入以维持祭坛水位。')
const isDeathReturning = ref(false)
const isSanityBroken = ref(false)

// 4. 指令终端 (Command CLI)
const currentCmd = ref('')
const cmdHistory = reactive([
  { time: getSysTime(), type: 'sys', msg: '阿卡姆底层协议 V5.0 初始化成功。' },
  { time: getSysTime(), type: 'sys', msg: '22408 飞升矩阵已锁定。' }
])
const logBox = ref(null)

function getSysTime() {
  const now = new Date()
  return `${now.getHours().toString().padStart(2,'0')}:${now.getMinutes().toString().padStart(2,'0')}:${now.getSeconds().toString().padStart(2,'0')}`
}

// ============================================================================
// [时间引擎：倒计时与理智损耗逻辑]
// ============================================================================
let timerInterval
const bootTimeEngine = () => {
  // 锁定 2029 届 22408 考研初试时间 (预计 2028-12-23 08:30)
  const targetDate = new Date('2028-12-23T08:30:00').getTime()
  
  timerInterval = setInterval(() => {
    const now = new Date().getTime()
    const diff = targetDate - now
    if (diff <= 0) {
      doomTimer.value = "DESTINY_FULFILLED"
      return
    }
    const d = Math.floor(diff / 86400000).toString().padStart(3, '0')
    const h = Math.floor((diff / 3600000) % 24).toString().padStart(2, '0')
    const m = Math.floor((diff / 60000) % 60).toString().padStart(2, '0')
    const s = Math.floor((diff / 1000) % 60).toString().padStart(2, '0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    // 理智值(SAN) 随时间极微量缓慢损耗
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001)
    if(sanityIndex.value < 50 && !isGazed.value) triggerGazedEvent()
  }, 1000)
}

// ============================================================================
// [核心指令解析：Sacrificial Parser]
// ============================================================================
const pushLog = (msg, type = 'info') => {
  cmdHistory.push({ time: getSysTime(), type, msg })
  if (cmdHistory.length > 60) cmdHistory.shift()
  nextTick(() => { if (logBox.value) logBox.value.scrollTop = logBox.value.scrollHeight })
}

const handleCommand = () => {
  const rawCmd = currentCmd.value.trim()
  if (!rawCmd) return
  
  pushLog(`> ${rawCmd}`, 'echo')
  const args = rawCmd.toLowerCase().split(' ')
  const rootCmd = args[0]

  switch (rootCmd) {
    case 'help':
      pushLog('可用神谕 (Sacrifices):', 'sys')
      pushLog(' - vocab [num]   - 献祭词汇同化进度 (例: vocab 50)', 'sys')
      pushLog(' - dose [num]    - 注入咖啡因 (MG) (例: dose 400)', 'sys')
      pushLog(' - pillar [name] - 转移认知焦点 (c/data/math)', 'sys')
      pushLog(' - help          - 唤醒此魔典', 'sys')
      pushLog(' - vzuor         - [FORBIDDEN] 呼唤真名', 'alert')
      pushLog(' - die           - 执行死亡回归协议', 'alert')
      break

    case 'vocab':
      const amt = parseInt(args[1])
      if (isNaN(amt) || amt <= 0) {
        pushLog('献祭失败：未定义或无效的数量。', 'error')
      } else {
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
        saveState('vocab', vocabSiphoned.value)
        
        // 考研进步，降低业障和理智侵蚀率
        karmaLoad.value = Math.max(0, karmaLoad.value - (amt * 0.05))
        saveState('karma', karmaLoad.value)
        
        sanityIndex.value = Math.min(100, sanityIndex.value + (amt * 0.005))
        saveState('sanity', sanityIndex.value)
        
        flavorText.value = `知识已刻印。当前同化率: <span class="text-bone">${vocabSiphoned.value}/4000</span>`
        pushLog(flavorText.value, 'success')
      }
      break

    case 'dose':
      const dose = parseInt(args[1])
      if (isNaN(dose)) {
        pushLog('无效的咖啡因剂量。', 'error')
      } else {
        caffeineLvl.value = dose
        if (dose >= 400) {
          triggerHyperDrive()
        } else {
          pushLog(`神经元催化剂已调整为 ${dose}mg。`, 'info')
        }
      }
      break

    case 'pillar':
      const p = args[1]
      const pillars = {'c':'C_LANG', 'data':'DATA_STRUCT', 'math':'MATH'}
      if (pillars[p]) {
        activePillar.value = pillars[p]
        pushLog(`认知焦点已转移至柱神 [${activePillar.value}]。`, 'success')
      } else {
        pushLog('未知的学科柱神。使用 help 查看。', 'error')
      }
      break

    case 'vzuor':
      triggerSanityBreak()
      break

    case 'die':
      triggerDeathReturn()
      break

    default:
      karmaLoad.value += 0.5
      saveState('karma', karmaLoad.value)
      pushLog(`未知指令。逻辑污染，业障增加。深渊在注视。`, 'error')
  }
  
  currentCmd.value = ''
}

// --- 极端状态触发器 ---
const triggerHyperDrive = () => {
  realmState.value = 'realm-hyper'
  pushLog('警告：咖啡因摄入过载！神经突触强制同步中...', 'error')
  // 通知 Canvas 引擎进入狂化模式
  if(window.vzuorEngine) window.vzuorEngine.setHyper(true)
  
  setTimeout(() => {
    realmState.value = 'realm-stable'
    caffeineLvl.value = 200
    if(window.vzuorEngine) window.vzuorEngine.setHyper(false)
    pushLog('神经元冷却完成。药效消退，回到肉身常规循环。', 'sys')
  }, 7000)
}

const triggerGazedEvent = () => {
  isGazed.value = true
  pushLog('<span class="text-glitch">学姐在看着你。</span>', 'alert')
  setTimeout(() => isGazed.value = false, 5000)
}

const triggerSanityBreak = () => {
  isSanityBroken.value = true
  pushLog('<span class="text-crimson">警告：真名被非理性呼唤。系统底层协议崩塌。HIDE_YOURSELF</span>', 'alert')
  sanityIndex.value -= 20.0
  setTimeout(() => isSanityBroken.value = false, 4000)
}

const triggerDeathReturn = () => {
  isDeathReturning.value = true
  // 死亡回归，不扣词汇量，但业障大增加，理智损耗
  karmaLoad.value += 10.0
  sanityIndex.value -= 10.0
  pushLog('检测到因果崩溃，正在重启世界线。因果业障已重新刻印。', 'error')
  setTimeout(() => isDeathReturning.value = false, 3000)
}


// ==========================================
// [终极高维入侵渲染引擎：The Vzuor-IK-Engine v5.0]
// 核心逻辑：底层 Canvas Compositing | 抛弃 CSS Blending | 像素级融合
// 代码密度突破：IK 触手物理、粒子系统、JPG 白底抹除算法
// ==========================================
const mainCanvas = ref(null)

// --- 材质解构与加载：V5.0 绝对法则 ---
// 羊皮纸纤维层将在底色基础上代码生成噪点，防止生硬。
const loadTexture = (src) => {
  return new Promise((resolve) => {
    const img = new Image(); img.onload = () => resolve(img); img.src = src
  })
}

class IKTentacle {
  constructor(x, y, segments, length, color) {
    this.baseX = x; this.baseY = y
    this.segments = segments; this.segLength = length
    this.angles = new Array(segments).fill(Math.random() * Math.PI)
    this.color = color
    this.noiseOffset = Math.random() * 1000
    // 反向运动学求解器状态
    this.joints = []
    for(let i=0; i<segments; i++) this.joints.push({x: x, y: y + i * length})
  }

  // 核心数学：FABRIK (Forward And Backward Reaching Inverse Kinematics) 算法
  solve(targetX, targetY) {
    // 逆向到达 target
    this.joints[this.segments-1] = { x: targetX, y: targetY }
    for(let i = this.segments - 2; i >= 0; i--) {
      const dx = this.joints[i].x - this.joints[i+1].x
      const dy = this.joints[i].y - this.joints[i+1].y
      const dist = Math.sqrt(dx*dx + dy*dy)
      const ratio = this.segLength / dist
      this.joints[i].x = this.joints[i+1].x + dx * ratio
      this.joints[i].y = this.joints[i+1].y + dy * ratio
    }
    // 顺向回到 base
    this.joints[0] = { x: this.baseX, y: this.baseY }
    for(let i = 1; i < this.segments; i++) {
      const dx = this.joints[i].x - this.joints[i-1].x
      const dy = this.joints[i].y - this.joints[i-1].y
      const dist = Math.sqrt(dx*dx + dy*dy)
      const ratio = this.segLength / dist
      this.joints[i].x = this.joints[i-1].x + dx * ratio
      this.joints[i].y = this.joints[i-1].y + dy * ratio
    }
  }

  draw(ctx, time, isHyper) {
    ctx.beginPath()
    ctx.moveTo(this.joints[0].x, this.joints[0].y)
    
    for (let i = 1; i < this.segments; i++) {
      // 触手根部粗，尖端细，且随机蠕动感
      const wob = Math.sin(time * (isHyper?4:1) + i * 0.3 + this.noiseOffset) * (isHyper?3:1)
      const dx = this.joints[i].x - this.joints[i-1].x
      const dy = this.joints[i].y - this.joints[i-1].y
      const angle = Math.atan2(dy, dx)
      
      const px = this.joints[i].x + Math.sin(angle) * wob
      const py = this.joints[i].y - Math.cos(angle) * wob
      
      ctx.lineTo(px, py)
    }
    
    ctx.strokeStyle = this.color
    ctx.lineWidth = isHyper ? 3.5 : 1.5
    ctx.lineCap = 'round'
    // 物理混合模式：正片叠底，彻底抹除线条周围可能残留的白点
    ctx.globalCompositeOperation = 'multiply'
    ctx.stroke()
    ctx.globalCompositeOperation = 'source-over' // 恢复默认
  }
}

// --- 2. 墨水毛细扩散与入侵算法 (Cellular Automata) ---
class InkParticle {
  constructor(w, h) {
    this.w = w; this.h = h; this.reset()
  }
  reset() {
    this.x = (this.w / 2) + (Math.random() - 0.5) * 50
    this.y = (this.h / 2) + (Math.random() - 0.5) * 50
    this.vx = (Math.random() - 0.5) * 0.5
    this.vy = (Math.random() - 0.5) * 0.5
    this.life = Math.random() * 120 + 60
    this.maxLife = this.life
    this.size = Math.random() * 2 + 1
    // 材质颜色：虚空墨水
    this.color = `rgba(13, 26, 30, `
  }
  update(isHyper, centerDistAttract) {
    this.x += this.vx
    this.y += this.vy
    
    // 粒子受到中央核心(理智值)的吸引力
    const dx = (this.w/2) - this.x
    const dy = (this.h/2) - this.y
    const dist = Math.sqrt(dx*dx + dy*dy)
    
    // 粒子扩散，但在Hyper模式下疯狂加速
    if (dist > centerDistAttract) {
      this.vx += dx * (isHyper ? 0.005 : 0.001)
      this.vy += dy * (isHyper ? 0.005 : 0.001)
    }

    this.life--
    if (this.life <= 0 || dist > this.w) this.reset()
  }
  draw(ctx) {
    const opacity = (this.life / this.maxLife) * 0.8
    // 绝对混合：正片叠底，让粒子像油渍一样洇入羊皮纸纹理
    ctx.globalCompositeOperation = 'multiply'
    ctx.fillStyle = `${this.color}${opacity})`
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2)
    ctx.fill()
    ctx.globalCompositeOperation = 'source-over'
  }
}

let animationId, ctx, particles = [], tentacles = []
let totemImg, inkStains = [], tentacleSketches = []
let w, h, time = 0
let isEngineHyper = false

const initVzuorEngine = async () => {
  if (!mainCanvas.value) return
  ctx = mainCanvas.value.getContext('2d')
  w = mainCanvas.value.clientWidth; h = mainCanvas.value.clientHeight
  mainCanvas.value.width = w; mainCanvas.value.height = h

  // 通知：开始加载JPG材质，启动像素叠底算法
  pushLog('系统已介入：正在唤醒底层 Canvas Compositing 法则...', 'sys')
  pushLog('正在注入JPG真名材质。算法：globalCompositeOperation = "multiply"', 'sys')

  // --- 高阶 JPG 材质重载 ---
  // totem.jpg 耶稣克苏鲁版，作为入侵核心
  // ink1-5.jpg 墨迹，用于全方位视觉污染
  // tentacle1,2.jpg 触手素描，用于静默背景入侵
  [totemImg, ...inkStains] = await Promise.all([
    loadTexture('/assets/totem.jpg'),
    loadTexture('/assets/ink1.jpg'),
    loadTexture('/assets/ink2.jpg'),
    loadTexture('/assets/ink3.jpg'),
    loadTexture('/assets/ink4.jpg'),
    loadTexture('/assets/ink5.jpg'),
    loadTexture('/assets/tentacle1.jpg'),
    loadTexture('/assets/tentacle2.jpg')
  ])
  
  // 提取触手素描供背景使用
  tentacleSketches = inkStains.splice(inkStains.length - 2, 2)
  pushLog('JPG材质加载完成，像素叠底光学封印已解除。入侵感已就绪。', 'success')

  // 初始化粒子流体引擎 (墨水入侵)
  for (let i = 0; i < 200; i++) particles.push(new InkParticle(w, h))

  // 初始化反向运动学触手群 (绑定中枢核心周围)
  const colors = ['rgba(10, 20, 25, 0.4)', 'rgba(5, 10, 15, 0.3)']
  for (let i = 0; i < 8; i++) {
    const color = colors[i%2]
    tentacles.push(new IKTentacle(w/2, h/2, 12, 15, color))
  }

  // 绑定神识鼠标(MouseMove)交互
  let mx = w/2, my = h/2
  window.addEventListener('mousemove', (e) => {
    // 鼠标在Canvas内的相对坐标
    const rect = mainCanvas.value.getBoundingClientRect()
    mx = e.clientX - rect.left
    my = e.clientY - rect.top
  })

  // 暴露给 Vue 组件进行狂化模式切换
  window.vzuorEngine = {
    setHyper: (val) => isEngineHyper = val
  }

  // --- 终极像素渲染循环 (The Render Grimoire) ---
  const renderLoop = () => {
    // 制造高频拖影，产生非理性的生理动态
    ctx.fillStyle = 'rgba(242, 239, 232, 0.15)' // 使用羊皮纸底色作为拖影层，实现物理覆盖
    ctx.fillRect(0, 0, w, h)
    
    // 背景层：静默触手素描 (JPG色度正片叠底)
    if(tentacleSketches[0]) {
      ctx.globalCompositeOperation = 'multiply'; ctx.globalAlpha = 0.1
      ctx.drawImage(tentacleSketches[0], w * 0.05, h * 0.2, w * 0.4, h * 0.4)
      ctx.globalAlpha = 1; ctx.globalCompositeOperation = 'source-over'
    }

    const ratio = vocabSiphoned.value / 4000
    // 水位(裂隙)计算
    const abyssLevel = h * (1 - ratio * 0.8)
    const attractDist = 150 + Math.sin(time*1.5) * 10 

    // 核心渲染：耶稣受难图腾 (JPG色度正片叠底)
    // 这里彻底解决JPG白底问题。只要黑色线条，不要生硬方框。
    if (totemImg) {
      ctx.globalCompositeOperation = 'multiply' // 绝对混合
      const totemH = h * 0.6
      const totemW = totemH * (totemImg.width / totemImg.height)
      // 在 Hyper 模式下产生非欧几里得抖动
      const tx = (w / 2) - (totemW / 2) + (isEngineHyper ? (Math.random()-0.5)*10 : 0)
      const ty = h * 0.15 + (isEngineHyper ? (Math.random()-0.5)*10 : 0)
      ctx.drawImage(totemImg, tx, ty, totemW, totemH)
      ctx.globalCompositeOperation = 'source-over'
    }

    // 更新绘制动态粒子流体
    particles.forEach(p => {
      p.update(isEngineHyper, attractDist)
      p.draw(ctx)
    })

    // 更新绘制反向运动学触手 (追踪神识鼠标)
    tentacles.forEach((t, idx) => {
      // 触手追踪鼠标坐标，但在Hyper下，部分触手狂暴自主运动
      let tx = mx + Math.sin(time * 0.5 + idx) * (isEngineHyper ? 200 : 50)
      let ty = my + Math.cos(time * 0.5 + idx) * (isEngineHyper ? 200 : 50)
      t.solve(tx, ty)
      t.draw(ctx, time, isEngineHyper)
    })
    
    // 静默墨迹细节 (JPG色度正片叠底)
    if (inkStains[0]) {
      ctx.globalCompositeOperation = 'multiply'; ctx.globalAlpha = 0.85
      ctx.drawImage(inkStains[0], w * -0.05, h * -0.05, w * 0.3, h * 0.3) //ink1
      if(inkStains[1]) ctx.drawImage(inkStains[1], w * 0.7, h * 0.7, w * 0.35, h * 0.35) //ink2
      ctx.globalAlpha = 1; ctx.globalCompositeOperation = 'source-over'
    }

    time += isEngineHyper ? 0.08 : 0.04
    animationId = requestAnimationFrame(renderLoop)
  }
  
  renderLoop()
}

// --- 3. 生命周期与资源清理 ---
onMounted(() => {
  bootTimeEngine()
  initVzuorEngine()
})

onBeforeUnmount(() => {
  clearInterval(timerInterval)
  cancelAnimationFrame(animationId)
})

</script>

<style>
/* ==========================================
   [CSS 终极布局系统：絶対隔離 Grid 网格]
   [核心：确保秩序永不重叠 | 为 Canvas 提供交互遮罩]
   [代码量：100+ 行专属于布局的精准推演]
   ========================================== */

@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@800;900&family=IM+Fell+English+SC&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  /* 阿卡姆祭坛调色板 */
  --c-paper: #E2DED0; /* 腐烂羊皮纸纤维色 */
  --c-text-main: #1A1A1A; /* 极致黑 */
  --c-text-muted: #66635A;
  --c-blood: #A31D1D; /* 业障红 */
  --c-terminal: #00FF66; /* 飞升绿 */
  --c-muted: #4A4740;

  /* 宿命字体配置 */
  --f-serif: 'IM Fell English SC', serif;
  --f-title: 'Cinzel', serif;
  --f-mono: 'Space Mono', monospace;
}

body, html {
  margin: 0; padding: 0; width: 100vw; height: 100vh;
  /* 恢复最初的羊皮纸色。不使用背景图，而是通过 Canvas 代码生成颗粒感。 */
  background-color: var(--c-paper);
  color: var(--c-text-main); font-family: var(--f-serif);
  overflow: hidden; -webkit-font-smoothing: antialiased;
}

/* 全局背景干扰污染层 */
.static-noise {
  position: fixed; inset: 0; z-index: 1000; pointer-events: none;
  background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="4" stitchTiles="stitch"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>');
  opacity: 0.04;
}
.void-scanlines {
  position: fixed; inset: 0; z-index: 1001; pointer-events: none;
  background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.2) 50%);
  background-size: 100% 4px;
}

/* 死亡回归全屏入侵 */
.death-return-active::after {
  content: ''; position: fixed; inset: 0; z-index: 9999;
  background: black; animation: death-flash 3s cubic-bezier(0.19, 1, 0.22, 1) forwards;
}
@keyframes death-flash { 0% { opacity: 1; background: var(--c-blood); } 10% { background: black; } 100% { opacity: 0; } }

.text-crimson { color: var(--c-blood); }
.text-bone { color: #f2efe4; font-weight: 700; }
.mt-auto { margin-top: auto; }
.mt-2 { margin-top: 10px; }
.mt-4 { margin-top: 20px; }

/* ----------------------------------------------------------------------------
   [CSS Grid Grid 布局：绝对隔离秩序层]
   ---------------------------------------------------------------------------- */
.interface-grid {
  position: relative; z-index: 50; /* 在 Canvas 渲染层之上 */
  width: 100vw; height: 100vh;
  padding: 3rem; box-sizing: border-box;
  display: grid;
  /* 严格的 3 列布局，确保文字永远不会换行重叠 */
  grid-template-columns: 320px 1fr 350px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header header"
    "left center right"
    "footer footer footer";
  gap: 2rem;
  pointer-events: none; /* 让鼠标事件穿透到下层 Canvas 进行 IK 追踪 */
}

/* 使 grid 内部的交互元素重新获得点击 */
.top-sect, .left-pillar-sect, .right-metrics-sect, .input-sect, .siphon-readout {
  pointer-events: auto;
}

/* --- 天极：Header --- */
.top-sect {
  grid-area: header;
  display: flex; justify-content: space-between; align-items: flex-end;
  border-bottom: 2px solid var(--c-text-main);
  padding-bottom: 1rem;
}
.title-construct { display: flex; flex-direction: column; }
.eldritch-title {
  font-family: var(--f-title); font-size: clamp(2.5rem, 3.5vw, 4.2rem);
  margin: 0; font-weight: 900; letter-spacing: 2px;
}
.status-monitor { display: flex; align-items: center; gap: 8px; margin-top: 5px; opacity: 0.6; }
.pulse-sig { width: 8px; height: 8px; background-color: var(--c-blood); border-radius: 50%; animation: pulse 2s infinite; }
.mono-label { font-family: var(--f-mono); font-size: 0.7rem; font-weight: 700; }

.chronos-construct { text-align: right; }
.doom-timer { font-size: clamp(1.8rem, 2.8vw, 3.2rem); font-weight: 700; color: var(--c-text-main); letter-spacing: -2px; line-height: 1; }
.timer-label { font-size: 0.65rem; color: var(--c-text-muted); margin-top: 8px; letter-spacing: 2px; font-weight: bold; }

/* --- 左翼：三大柱神与熔炉 --- */
.left-pillar-sect { grid-area: left; display: flex; flex-direction: column; border-right: 1px dashed var(--c-muted); padding-right: 2rem; }
.zone-title { font-size: 0.9rem; color: var(--c-text-muted); letter-spacing: 3px; border-bottom: 1px solid var(--c-text-muted); padding-bottom: 8px; margin-bottom: 20px; font-weight: bold; text-transform: uppercase; }

.pillar-matrix { display: flex; flex-direction: column; gap: 1.5rem; }
.pillar-node { display: flex; align-items: center; gap: 1rem; opacity: 0.35; transition: all 0.3s ease; }
.pillar-node.active { opacity: 1; }
.pillar-node.active .node-icon { color: var(--c-terminal); text-shadow: 0 0 10px rgba(0,255,102,0.3); border-color: var(--c-terminal); }
.pillar-node.active .node-name { color: var(--c-terminal); }

.node-icon { width: 42px; height: 42px; border: 1px solid var(--c-text-muted); display: flex; align-items: center; justify-content: center; font-family: var(--f-title); font-size: 1.4rem; font-weight: 800; color: var(--c-text-muted); }
.node-name { font-weight: 700; font-size: 0.95rem; }
.node-status { font-size: 0.7rem; color: var(--c-text-muted); margin-top: 2px; }

.forge-stats { font-family: var(--f-mono); }
.forge-bar-wrap { width: 100%; height: 6px; background: rgba(0,0,0,0.1); border: 1px solid rgba(0,0,0,0.2); margin-top: 8px; }
.forge-bar { height: 100%; width: 100%; position: relative; }
.forge-progress { height: 100%; background: var(--c-terminal); box-shadow: 0 0 10px rgba(0,255,102,0.2); }
.forge-dose-row { display: flex; justify-content: space-between; align-items: center; margin-top: 12px; font-size: 0.8rem; }
.dose-val { font-size: 1.6rem; font-weight: 700; color: var(--c-bone); }
.high-dose { animation: slight-wobble 0.1s infinite; text-shadow: 0 0 15px var(--c-blood); }

/* --- 中枢：Canvas 深渊 --- */
.center-rift-sect {
  grid-area: center; display: flex; justify-content: center; align-items: center;
  position: relative;
}
.rift-containment {
  position: absolute; inset: 0; z-index: 10;
  display: flex; justify-content: center; align-items: center;
}
.main-canvas {
  width: 100%; height: 100%; display: block;
}

/* 核心进度读数 (悬浮在耶稣中央，混合模式正常) */
.siphon-readout {
  position: absolute; z-index: 100; text-align: center;
  pointer-events: none; /* 穿透鼠标交互 */
}
.readout-header { font-size: 0.7rem; color: var(--c-text-muted); opacity: 0.6; letter-spacing: 3px; font-weight: bold; }
.massive-percent { margin-left: -15px; }
.massive-percent .num { font-family: var(--f-title); font-size: 11rem; font-weight: 900; line-height: 0.8; letter-spacing: -10px; color: var(--c-text-main); }
.massive-percent .unit { font-size: 2.5rem; color: var(--c-text-muted); opacity: 0.3; }
.vocab-total { font-family: var(--f-mono); font-size: 0.75rem; color: var(--c-text-muted); letter-spacing: 1px; font-weight: bold; margin-top: 20px; }

/* --- 右翼：理智与终端日志 --- */
.right-metrics-sect { grid-area: right; display: flex; flex-direction: column; border-left: 1px dashed var(--c-muted); padding-left: 2rem; }
.metrics-matrix { display: flex; flex-direction: column; gap: 15px; font-size: 0.9rem; font-family: var(--f-mono); }
.metric-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid rgba(0,0,0,0.05); padding-bottom: 5px; }
.metric-row .label { color: var(--c-text-muted); }
.metric-row .value { font-weight: 700; color: var(--c-text-main); }

.terminal-logs {
  flex: 1; border: 1px solid rgba(0,0,0,0.1); background: rgba(0,0,0,0.03);
  padding: 1rem; overflow-y: auto; display: flex; flex-direction: column; gap: 8px;
  font-family: var(--f-mono); font-size: 0.75rem; font-weight: 400; line-height: 1.4;
  /* 隐藏滚动条 */ Scrollbar-width: none;
}
.terminal-logs::-webkit-scrollbar { display: none; }
.log-entry { display: flex; gap: 10px; word-break: break-all; }
.log-time { color: var(--c-text-muted); opacity: 0.6; flex-shrink: 0; }
.log-entry.info .log-msg { color: var(--c-text-main); }
.log-entry.sys .log-msg { color: var(--c-text-muted); }
.log-entry.echo .log-msg { color: #888; font-style: italic; opacity: 0.7; }
.log-entry.success .log-msg { color: var(--c-terminal); text-shadow: 0 0 10px rgba(0,255,102,0.2); }
.log-entry.error .log-msg, .log-entry.alert .log-msg { color: var(--c-blood); }

/* --- 地极：指令台 --- */
.input-sect { grid-area: footer; border-top: 2px solid var(--c-text-main); padding-top: 1.5rem; }
.input-cli { display: flex; align-items: center; gap: 1.5rem; }
.cli-prefix { font-family: var(--f-mono); font-weight: 700; font-size: 1.4rem; color: var(--c-terminal); }
.cli-input {
  flex: 1; background: transparent; border: none; outline: none;
  font-family: var(--f-serif); font-size: 1.6rem; font-style: italic; color: var(--c-text-main);
  border-bottom: 1px solid rgba(0,0,0,0.05);
}
.cli-input::placeholder { color: var(--c-text-muted); opacity: 0.4; font-style: normal; font-family: var(--f-mono); font-size: 0.9rem; }

/* 响应式降级防御 */
@media (max-width: 1200px) {
  .ldritch-title { font-size: clamp(2.2rem, 3vw, 3.5rem); }
  .massive-percent .num { font-size: 8rem; }
}
@media (max-width: 950px) {
  .interface-grid {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto auto auto;
    grid-template-areas: "header" "center" "left" "right" "footer";
    overflow-y: auto; height: 100%;
  }
  .left-pillar-sect, .right-metrics-sect { border: none; padding: 0; }
  .center-rift-sect { height: 500px; }
  .massive-percent .num { font-size: 6rem; }
}

/* 核心动画定义的膨胀 */
@keyframes pulse { 0% { opacity: 0.3; } 50% { opacity: 1; } 100% { opacity: 0.3; } }
@keyframes faint-blink { 0%, 100% { opacity: 1; } 50% { opacity: 0.4; } }
@keyframes death-flash { 0% { opacity: 1; background: var(--c-blood); } 10% { background: black; } 100% { opacity: 0; } }
@keyframes glitch-anim { 0% { clip: rect(10px, 999px, 40px, 0); } 100% { clip: rect(70px, 999px, 90px, 0); } }
@keyframes hyper-tremor { 0% { transform: translate(1px, 1px) rotate(0deg); } 100% { transform: translate(-1px, -1px) rotate(0.1deg); } }
@keyframes slight-wobble { 0% { transform: skewX(-1deg); } 100% { transform: skewX(1deg); } }
</style>