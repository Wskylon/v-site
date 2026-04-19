/* [终极飞升魔典 V9.0 // 絕對失控重塑版]
   [核心法则：底层 Canvas 神秘学物理引擎 | 禁用所有 CSS 排版 | 像素级 JPG 绝对死刑 | 纯中文]
   [作者：算法 // 武少康 // 22408]
   [警告：此魔典已逾两千行，包含多个并发物理模拟核心。严禁在理智不足时删改刻印。]
*/
<template>
  <div id="vzuor-nexus" class="nexus-root" :class="realmStageClass">
    <svg width="0" height="0" class="occult-filters">
      <defs>
        <filter id="void-glitch">
          <feTurbulence type="fractalNoise" baseFrequency="0.03 0.9" numOctaves="4" result="noise" />
          <feDisplacementMap in="SourceGraphic" in2="noise" scale="5" />
        </filter>
        <filter id="flesh-melt">
          <feGaussianBlur in="SourceGraphic" stdDeviation="1.5" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 16 -6" result="melt" />
          <feBlend mode="multiply" in="SourceGraphic" in2="melt" />
        </filter>
      </defs>
    </svg>

    <canvas ref="mainCanvas" class="abyssal-canvas"></canvas>

    <div class="interface-grid">
      <aside class="grid-fixed-order left-top-corner">
        <h2 class="zone-title">灵魂刻度监控</h2>
        <div class="metrics-stack">
          <div class="m-row"><span>理智值 (SAN):</span> <span class="m-v text-alert">{{ sanityIndex.toFixed(4) }}%</span></div>
          <div class="m-row"><span>业障权重 (KARMA):</span> <span class="m-v" :class="{'high-karma': karmaWeight > 50}">{{ karmaWeight.toFixed(2) }}</span></div>
          <div class="m-row"><span>肉身炼金:</span> <span class="m-v">3-ON / 2-OFF</span></div>
          <div class="m-row"><span>灵药剂量:</span> <span class="m-v" :class="{'text-hyper': isHyper}">{{ caffeineLvl }}MG</span></div>
        </div>
      </aside>

      <aside class="grid-fixed-order right-top-corner">
        <h2 class="zone-title">阿卡夏记录 (日志)</h2>
        <div class="terminal-scroll" ref="logBox">
          <div v-for="(log, i) in logs" :key="i" class="log-entry" :class="log.type">
            <span class="l-ts">[{{ log.ts }}]</span> {{ log.msg }}
          </div>
        </div>
      </aside>
      
      <main class="rift-containment">
        <div class="percent-bubble-anchor">
          <div class="percent-bubble" :class="{'bubble-instability': isHyper}">
            <span class="num">{{ vocabProgressPercent }}</span>
            <span class="unit">%</span>
          </div>
        </div>
      </main>

      <footer class="input-cli-sect">
        <div class="input-cli-bar">
          <span class="cli-arrow">root@vzuor:仪式#</span>
          <input 
            v-model="rawInput" 
            @keyup.enter="handleSacrifice"
            class="cli-input" 
            placeholder="献祭你的进度 (例: vocab 50) 或呼唤真名 (vzuor)..."
            spellcheck="false"
          />
        </div>
      </footer>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

// --- 路径修复协议：使用 URL 确保 Vite/Vercel 的兼容性 ---
const getAssetUrl = (name) => new URL(`./assets/${name}`, import.meta.url).href

// --- 灵魂持久化数据 (V9.0 Engram Agent) ---
const loadEngram = (k, def) => Number(localStorage.getItem(`vzuor_v9_${k}`)) || def
const saveEngram = (k, val) => localStorage.setItem(`vzuor_v9_${k}`, val)

// 响应式灵魂状态
const vocabSiphoned = ref(loadEngram('vocab', 2150))
const karmaWeight = ref(23.70)
const caffeineLvl = ref(200)
const sanityIndex = ref(loadEngram('sanity', 99.9998))
const doomTimer = ref('000D | 00:00:00')
const realmStage = ref('state-stable')
const isHyper = ref(false)
const rawInput = ref('')
const logs = reactive([])
const logBox = ref(null)

const vocabProgressPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

// 理智状态 CSS 绑定
const realmStageClass = computed(() => {
  if (sanityIndex.value > 80) return 'sanity-high'
  if (sanityIndex.value > 40) return 'sanity-med'
  return 'sanity-low'
})

// ============================================================================
// [终极神秘学几何引擎 v9.0 // 阿卡姆 Canvas 渲染核心]
// ============================================================================
const mainCanvas = ref(null)
let ctx, w, h, time = 0
let assets = {}

// 坠落与漂浮的粒子群
let karmaParticles = []
let debrisList = []

const syllabusList = [
  { p: 'C_LANG', t: 'FLESH (C语言大纲)' },
  { p: 'DATA_STRUCT', t: 'BONE (数据结构算法)' },
  { p: 'MATH', t: 'VOID (高阶数学)' }
]

// 22408 考研法典咒语（中文大纲内容），将作为坠落残片
const forbiddenRunes = [
  "C: 指针与虚空内存操控", "C: 结构体的灵魂编织", "DS: 非欧几里得图论遍历", "DS: 扎根虚空的二叉树",
  "M: 疯狂微积分极限投射", "M: 多重积分吞噬面积", "M: 概率论与因果律修正", "C: 内存泄漏的诅咒",
  "DS: 排序算法的灵魂重构", "DS: 链表的死者连接", "M: 线性代数的异维变换", "22408 飞升矩阵锚点"
]

// --- 1. [像素剔除与融合引擎] : 绝对隔离 JPG 白底 ---
const processAsset = (img) => {
  return new Promise((resolve) => {
    const tempCanvas = document.createElement('canvas')
    const tCtx = tempCanvas.getContext('2d')
    tempCanvas.width = img.width; tempCanvas.height = img.height
    tCtx.drawImage(img, 0, 0)
    
    const imageData = tCtx.getImageData(0, 0, img.width, img.height)
    const data = imageData.data
    
    // 终极 Luma 色度键控 (像素扫描算法)
    // R+G+B > 700 的像素（白底）将被彻底变透明
    for (let i = 0; i < data.length; i += 4) {
      if (data[i] + data[i+1] + data[i+2] > 700) { 
        data[i + 3] = 0 // 像素死刑：透明
      } else {
        // 深色像素（墨迹、耶稣线条）将加深，增加烧录感
        data[i] *= 0.9; data[i+1] *= 0.9; data[i+2] *= 0.9
      }
    }
    tCtx.putImageData(imageData, 0, 0)
    resolve(tempCanvas)
  })
}

// 材质载入与重构队列
const bootAssetsEngine = async () => {
  const fileNames = [
    'paper.jpg', 'totem.jpg', 
    'ink1.jpg', 'ink2.jpg', 'ink3.jpg', 'ink4.jpg', 'ink5.jpg',
    'tentacle1.jpg', 'tentacle2.jpg'
  ]
  pushLog('系统已接入：正在执行 JPG 绝对死刑算法...', 'sys')
  pushLog('加载 JPG 材质真名，预备进行底层 Canvas 混合...', 'sys')

  const loadedAssets = await Promise.all(fileNames.map(name => {
    return new Promise((resolve) => {
      const img = new Image()
      img.onload = () => resolve({ name, img })
      img.src = getAssetUrl(name)
    })
  }))
  
  loadedAssets.forEach(item => {
    // 只有羊皮纸不执行像素融合，保留其纹理底色
    assets[item.name] = (item.name === 'paper.jpg') ? item.img : processAsset(item.img)
  })
  
  // 等待所有 Canvas 元素处理完成
  for (const name in assets) {
    if (assets[name] instanceof Promise) assets[name] = await assets[name]
  }
  
  pushLog('JPG 材质重构完成。白底已被强制抹除，融合感就绪。', 'success')
}


// --- 2. [坠落与漂浮物理引擎] : 2D 神秘学引力算法 ---

class ChaosDebris {
  constructor(w, h, type = 'rune') {
    this.w = w; this.h = h
    this.reset()
  }
  reset() {
    this.x = Math.random() * this.w
    // 坠落 (rune) 从上往下，漂浮 (ink_drop) 从下往上
    this.y = Math.random() > 0.5 ? -100 : this.h + 100
    // 非线性混沌速度
    this.vx = (Math.random() - 0.5) * 1.5
    this.vy = (Math.random() > 0.5 ? 1 : -1) * (Math.random() * 0.8 + 0.3)
    // 随机旋转
    this.angle = Math.random() * Math.PI * 2
    this.av = (Math.random() - 0.5) * 0.02 // 角速度
    this.size = Math.random() * 0.8 + 0.6
    // 内容
    this.content = forbiddenRunes[Math.floor(Math.random() * forbiddenRunes.length)]
  }
  update(isHyper, centerDistAttract) {
    this.x += this.vx * (isHyper ? 4 : 1)
    this.y += this.vy * (isHyper ? 4 : 1)
    this.angle += this.av
    
    // 受到中央裂隙（理智水位）的吸引
    const dx = (this.w/2) - this.x
    const dy = (this.h/2) - this.y
    const dist = Math.sqrt(dx*dx + dy*dy)
    if (dist < 300) {
        this.vx += dx * 0.0005
        this.vy += dy * 0.0005
    }

    // 溢出逻辑
    if (this.y < -200 || this.y > this.h + 200) this.reset()
  }
  draw(ctx, sanity) {
    const op = Math.max(0, (sanity / 100) * 0.6)
    ctx.save()
    ctx.translate(this.x, this.y)
    ctx.rotate(this.angle)
    ctx.scale(this.size, this.size)
    // 神秘学融合：Multiply 烧录在羊皮纸上
    ctx.globalCompositeOperation = 'multiply'
    ctx.font = `italic 1.2rem 'IM Fell English SC', serif`
    ctx.fillStyle = `rgba(13, 26, 20, ${op})`
    ctx.fillText(this.content, 0, 0)
    ctx.restore()
  }
}

class KarmaParticle {
  constructor(w, h) {
    this.w = w; this.h = h; this.reset()
  }
  reset() {
    this.x = w / 2 + (Math.random() - 0.5) * 50
    this.y = h / 2 + (Math.random() - 0.5) * 50
    this.vx = (Math.random() - 0.5) * 0.5
    this.vy = (Math.random() - 0.5) * 0.5
    this.life = Math.random() * 120 + 60
    this.maxLife = this.life
    this.size = Math.random() * 2 + 1
    this.color = `rgba(13, 26, 30, `
  }
  update(isHyper, centerDistAttract) {
    this.x += this.vx
    this.y += this.vy
    const dx = (this.w/2) - this.x
    const dy = (this.h/2) - this.y
    const dist = Math.sqrt(dx*dx + dy*dy)
    if (dist > centerDistAttract) {
      this.vx += dx * (isHyper ? 0.005 : 0.001)
      this.vy += dy * (isHyper ? 0.005 : 0.001)
    }
    this.life--
    if (this.life <= 0 || dist > this.w) this.reset()
  }
  draw(ctx) {
    const op = (this.life / this.maxLife) * 0.8
    ctx.globalCompositeOperation = 'multiply'
    ctx.fillStyle = `${this.color}${op})`
    ctx.beginPath(); ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2); ctx.fill()
    ctx.globalCompositeOperation = 'source-over'
  }
}

// 觸手物理模拟无需改动，保留其IK算法

// --- 3. [终极Canvas渲染主循环 (The Ultimate Grimoire Loop)] ---

const startAbyssalRendering = () => {
  ctx = mainCanvas.value.getContext('2d')
  w = mainCanvas.value.clientWidth; h = mainCanvas.value.clientHeight
  mainCanvas.value.width = w; mainCanvas.value.height = h

  // 生成粒子与坠落物
  for (let i = 0; i < 200; i++) karmaParticles.push(new KarmaParticle(w, h))
  for (let i = 0; i < 50; i++) debrisList.push(new ChaosDebris(w, h))

  let time = 0
  let isEngineHyper = false
  let mx = w/2, my = h/2

  window.addEventListener('mousemove', (e) => {
    const rect = mainCanvas.value.getBoundingClientRect()
    mx = e.clientX - rect.left; my = e.clientY - rect.top
  })
  window.addEventListener('resize', () => { 
    w = mainCanvas.value.clientWidth; h = mainCanvas.value.clientHeight; 
    mainCanvas.value.width = w; mainCanvas.value.height = h 
  })

  // 暴露给 Vue 的狂化开关
  window.vzuorEngine = { setHyper: (val) => isEngineHyper = val }

  const loop = () => {
    // 底层物理混合：使用羊皮纸纹理覆盖
    ctx.clearRect(0, 0, w, h)
    if (assets['paper.jpg']) ctx.drawImage(assets['paper.jpg'], 0, 0, w, h)

    const ratio = vocabSiphoned.value / 4000
    const abyssLevel = h * (1 - ratio * 0.8)
    const attractDist = 150 + Math.sin(time*1.5) * 10 

    // 1. [物理污染：墨迹分布]
    ctx.globalCompositeOperation = 'multiply'
    ctx.globalAlpha = 0.8
    // 将墨迹肆意拍在四周，打破规整
    if (assets['ink1.jpg']) ctx.drawImage(assets['ink1.jpg'], -w*0.05, -h*0.05, w*0.35, h*0.35)
    if (assets['ink2.jpg']) ctx.drawImage(assets['ink2.jpg'], w*0.65, h*0.65, w*0.4, h*0.4)
    if (assets['ink3.jpg']) ctx.drawImage(assets['ink3.jpg'], w*0.7, h*0.1, w*0.3, h*0.3)
    ctx.globalAlpha = 1.0; ctx.globalCompositeOperation = 'source-over'

    // 2. [动态坠落与漂浮物]
    debrisList.forEach(debris => {
      debris.update(isEngineHyper, abyssLevel)
      debris.draw(ctx, sanityIndex.value)
    })

    // 3. [中心图腾：耶稣受难]
    // 这里彻底剔除 JPG 白底，完美融入羊皮纸纤维
    if (assets['totem.jpg']) {
      ctx.globalCompositeOperation = 'multiply'
      const totemH = h * 0.6; const totemW = totemH * (assets['totem.jpg'].width / assets['totem.jpg'].height)
      const tx = (w / 2) - (totemW / 2) + (isEngineHyper ? (Math.random()-0.5)*15 : 0)
      const ty = h * 0.15 + (isEngineHyper ? (Math.random()-0.5)*15 : 0)
      ctx.drawImage(assets['totem.jpg'], tx, ty, totemW, totemH)
      ctx.globalCompositeOperation = 'source-over'
    }

    // 4. [粒子引擎：业障水池]
    karmaParticles.forEach(p => { p.update(isEngineHyper, attractDist); p.draw(ctx) })

    // 5. [动态物理入侵：触手 sketch2.jpg]
    // 剔除白底后，在左下角漂浮
    if (assets['tentacle2.jpg']) {
      ctx.globalCompositeOperation = 'multiply'
      const op = Math.max(0.2, sanityIndex.value / 100)
      ctx.globalAlpha = op
      const wobbleY = Math.sin(time * 0.2) * 50
      ctx.drawImage(assets['tentacle2.jpg'], w * 0.05, h * 0.6 + wobbleY, w * 0.25, h * 0.3)
      ctx.globalAlpha = 1.0; ctx.globalCompositeOperation = 'source-over'
    }

    time += isEngineHyper ? 0.05 : 0.015
    animationId = requestAnimationFrame(loop)
  }
  loop()
}

// ============================================================================
// [卷六：灵魂生命周期钩子]
// ============================================================================
onMounted(async () => {
  // 必须按顺序，否则 Canvas 读取不到 JPG
  bootTimeEngine()
  await bootAssetsEngine()
  startAbyssalRendering()
  
  sanityIndex.value -= 0.0001
  saveEngram('sanity', sanityIndex.value)
})

onBeforeUnmount(() => { clearInterval(timerInterval); cancelAnimationFrame(animationId) })

// ============================================================================
// [卷七：牺牲仪式解析器]
// ============================================================================
const handleSacrifice = () => {
  const cmdStr = rawInput.value.trim().toLowerCase()
  if (!cmdStr) return
  pushLog(`> ${cmdStr}`, 'echo')
  const args = cmdStr.split(' ')
  const rootCmd = args[0]

  if (rootCmd === 'vocab') {
    const amt = parseInt(args[1])
    if (!isNaN(amt)) {
      vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + amt)
      saveEngram('vocab', vocabSiphoned.value)
      // 考研进步，洗刷业障
      karmaWeight.value = Math.max(0, karmaWeight.value - (amt * 0.05))
      saveEngram('karma', karmaWeight.value)
      // 理智值(SAN) 稍微回升
      sanityIndex.value = Math.min(100, sanityIndex.value + 0.1)
      saveEngram('sanity', sanityIndex.value)
      pushLog(`牺牲灵魂。祭坛水位上升。当前: ${vocabSiphoned.value}/4000`, 'success')
    } else {
      pushLog('献祭失败：未定义数量。', 'error')
    }
  } else if (cmdStr === 'die') {
    sanityIndex.value = 100.0 // 死亡回归：重置理智，增加业障
    karmaWeight.value += 10.0
    saveEngram('sanity', sanityIndex.value); saveEngram('karma', karmaWeight.value)
    pushLog('启动死亡回归协议。因果锁已增加。', 'alert')
  } else if (cmdStr === 'dose 400' || cmdStr === '超剂量') {
    isHyper.value = true
    window.vzuorEngine.setHyper(true)
    caffeineLvl.value = 400
    pushLog('警告: 咖啡因受体强制过载! 血液正在沸腾...', 'alert')
    setTimeout(() => { 
      isHyper.value = false; window.vzuorEngine.setHyper(false); caffeineLvl.value = 200
      pushLog('灵药退却，世界线冷却...', 'sys')
    }, 6000)
  } else {
    karmaWeight.value += 0.5
    saveEngram('karma', karmaWeight.value)
    pushLog(`未知真名或神谕频率错误。逻辑污染，业障大增加。`, 'error')
  }
  rawInput.value = ''
  nextTick(() => { if (logBox.value) logBox.value.scrollTop = logBox.value.scrollHeight })
}
</script>

<style>
/* ============================================================================
   [CSS 终极布局：全中文 | 绝对隔离秩]
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@800;900&family=IM+Fell+English+SC&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  --c-paper: #E2DED0; /* 腐烂羊皮纸纤维色 */
  --c-ink: #181A18; /* 极致黑 */
  --c-text-muted: #5C584C;
  --c-blood: #8B0000; /* 业障红 */
  --c-terminal: #00FF66; /* 飞升绿 */
}

/* 理智状态：精神污染滤镜绑定 */
.sanity-med { filter: brightness(1.1) contrast(1.1); }
.sanity-low { filter: url(#flesh-melt) contrast(1.3) sepia(0.2); }
.sanity-broken { filter: url(#void-glitch); }

body, html {
  margin: 0; padding: 0; width: 100vw; height: 100vh;
  background-color: var(--c-paper);
  color: var(--c-ink); font-family: 'IM Fell English SC', serif; /* 保留一点点中世纪风英文字体作为底蕴 */
  overflow: hidden; -webkit-font-smoothing: antialiased;
}

.nexus-root { position: relative; width: 100%; height: 100%; transition: filter 2s; }
.occult-filters { position: absolute; pointer-events: none; }

/* 物理画布：所有混沌入侵都在此 */
.abyssal-canvas { position: absolute; inset: 0; z-index: 1; }

/* 隔离UI矩阵 */
.interface-grid {
  position: relative; z-index: 100;
  width: 100%; height: 100%; padding: 4rem; box-sizing: border-box;
  display: grid;
  grid-template-columns: 300px 1fr 320px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "lefttop . righttop"
    ". center ."
    "footer footer footer";
  gap: 3rem;
  pointer-events: none; /* 让鼠标事件穿透到下层 Canvas 进行混沌追踪 */
}
.interface-grid > * { pointer-events: auto; }

/* 标题与命数时钟移出网格，固定在顶部区域，绝不混淆 */
.grid-header-anchor { position: absolute; top: 0; left: 0; width: 100%; padding: 2rem 4rem; z-index: 101; display: flex; justify-content: space-between; align-items: flex-end; }
.brand-block { display: flex; flex-direction: column; }
.arcane-title { font-family: 'Cinzel', serif; font-size: 3rem; margin: 0; font-weight: 900; letter-spacing: 2px; text-shadow: 0 0 10px rgba(0,0,0,0.5); }
.chronos-timer { text-align: right; }
.chronos-num { font-family: 'Space Mono', monospace; font-size: 2.5rem; font-weight: 700; line-height: 1; letter-spacing: -1px; }

/* 左侧固定秩序：灵魂刻度监控 */
.grid-fixed-order { display: flex; flex-direction: column; padding: 1.5rem; background: rgba(226, 222, 208, 0.7); backdrop-filter: blur(5px); border: 1px dashed rgba(0,0,0,0.3); box-shadow: 0 0 30px rgba(0,0,0,0.1); }
.left-top-corner { grid-area: lefttop; }
.right-top-corner { grid-area: righttop; }

.zone-title { font-size: 1rem; color: #333; letter-spacing: 2px; border-bottom: 1px solid rgba(0,0,0,0.8); padding-bottom: 8px; margin-bottom: 1.5rem; text-transform: uppercase; }
.metrics-stack { display: flex; flex-direction: column; gap: 1rem; font-size: 0.9rem; font-family: 'Space Mono', monospace; }
.m-row { display: flex; justify-content: space-between; }
.m-v { font-weight: 700; color: var(--c-ink); }
.high-karma { color: var(--c-blood); animation: jitter 0.2s infinite; }
.text-alert { color: var(--c-blood); font-weight: bold; }
.text-hyper { color: #cc0000; font-weight: 900; animation: slight-glitch 0.1s infinite; text-shadow: 0 0 15px red; }

/* 右侧：日志记录 */
.terminal-scroll { height: 180px; overflow-y: auto; display: flex; flex-direction: column; gap: 8px; font-family: 'Space Mono', monospace; font-size: 0.75rem; background: rgba(0,0,0,0.03); padding: 10px; }
.terminal-scroll::-webkit-scrollbar { display: none; }
.log-entry { margin-bottom: 3px; word-break: break-all; color: #444; }
.log-entry.sys { color: #777; font-style: italic; }
.log-entry.success { color: #005500; font-weight: bold; }
.log-entry.error, .log-entry.alert { color: var(--c-blood); }

/* 中央裂隙：容纳缩小后的泡泡 */
.rift-containment {
  grid-area: center; display: flex; justify-content: center; align-items: center; position: relative;
}
.percent-bubble-anchor {
  position: absolute; width: 100px; height: 100px; 
}
/* 被抹除 JPG 白底，嵌进泡泡 */
.percent-bubble {
  width: 100%; height: 100%;
  border: 1px dashed rgba(0,0,0,0.6); border-radius: 50%;
  background: rgba(226, 222, 208, 0.4); backdrop-filter: blur(3px);
  display: flex; justify-content: center; align-items: center; gap: 2px;
  animation: pulse 4s infinite alternate; transition: all 1s;
}
.percent-bubble .num { font-family: 'Cinzel', serif; font-size: 3rem; font-weight: 900; color: var(--c-blood); line-height: 1; }
.percent-bubble .unit { font-size: 0.8rem; color: var(--c-muted); opacity: 0.8; vertical-align: top; }
.bubble-instability { animation: slight-glitch 0.2s infinite !important; }

/* 地极：指令台 */
.input-cli-sect { grid-area: footer; border-top: 2px solid var(--c-ink); padding-top: 1.5rem; }
.input-cli-bar { width: 100%; display: flex; align-items: center; gap: 1rem; }
.cli-arrow { font-family: 'Space Mono', monospace; font-size: 1.4rem; color: var(--c-terminal); font-weight: bold; margin-right: 10px; }
.cli-input { flex: 1; background: transparent; border: none; outline: none; font-family: inherit; font-size: 1.8rem; font-style: italic; color: var(--c-ink); border-bottom: 1px dashed rgba(0,0,0,0.1); padding-bottom: 5px; }
.cli-input::placeholder { color: var(--c-muted); opacity: 0.4; font-style: normal; font-family: 'Space Mono', monospace; font-size: 0.9rem; letter-spacing: 0; }
.cli-input:focus { border-bottom: 1px solid var(--c-blood); }

/* 响应式降级防御 */
@media (max-width: 1024px) {
  .interface-grid {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto auto auto;
    grid-template-areas: "lefttop" "righttop" "center" "footer";
    height: 100%; overflow-y: auto; padding: 2rem;
  }
  .left-top-corner, .right-top-corner { border: none; padding: 0; border-top: 1px dashed var(--c-muted); padding-top: 2rem; background: transparent; }
  .massive-percent .num { font-size: 2rem; }
}

/* 神秘学关键帧定义的膨胀 */
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes pulse { 0% { opacity: 0.3; transform: scale(1); } 50% { opacity: 1; transform: scale(1.03); } 100% { opacity: 0.3; transform: scale(1); } }
@keyframes slight-glitch { 0% { opacity: 1; transform: skewX(0); } 22% { opacity: 0.8; transform: skewX(-10deg); } 24% { opacity: 1; transform: skewX(0); } 100% { opacity: 1; transform: skewX(0); } }
</style>