/* ==========================================================================================
   [阿卡姆终极飞升魔典 V10.0 // 万物归一者 THE ONE IN ALL]
   [宿主：武少康 // 目标：22408 飞升 // 精神状态：临界]
   [融合协议：
    1. 纯中文交互与阿卡夏记录
    2. 光学正片叠底 (Multiply) 绝对抹杀 JPG 白底，确保素材 100% 渲染
    3. Canvas 2D 物理引擎：反向运动学触手 (IK) + 万有引力坠落符文 + 虚空流体粒子
    4. 反常规布局：UI 碎片化漂浮，核心祭坛居中，不再有规整的 Excel 边框
    5. 进度水滴：53% 被压缩并封印在深渊水泡中
   ========================================================================================== */
<template>
  <div id="vzuor-ascension" class="nexus-chaos-root" :class="sanityLevel">
    
    <img :src="imgPaper" class="bg-paper-texture" alt="paper" />
    <div class="static-noise-overlay"></div>

    <div class="optical-corruption">
      <img :src="imgInk1" class="stain s-ink1" />
      <img :src="imgInk2" class="stain s-ink2" />
      <img :src="imgInk3" class="stain s-ink3" />
      <img :src="imgInk4" class="stain s-ink4" />
      <img :src="imgInk5" class="stain s-ink5" />
      <img :src="imgTen1" class="stain s-ten1" />
      <img :src="imgTen2" class="stain s-ten2" />
    </div>

    <canvas ref="voidCanvas" class="abyssal-physics-canvas"></canvas>

    <main class="altar-core">
      <img :src="imgTotem" class="totem-jesus" :class="{'tremor': isHyper}" />
      
      <div class="void-orb-container">
        <div class="void-orb">
          <div class="water-level" :style="{ height: vocabPercent + '%' }"></div>
          <div class="orb-data">
            <span class="num">{{ vocabPercent }}</span><span class="pct">%</span>
          </div>
        </div>
        <div class="orb-label">{{ vocabSiphoned }} / 4000 灵魂</div>
      </div>
    </main>

    <div class="shattered-ui-layer">
      
      <div class="shard shard-top-left">
        <h1 class="chaotic-title" data-text="V'ZUOR KHAA-SH'AN">V'ZUOR KHAA-SH'AN</h1>
        <div class="doom-clock">
          <span class="clock-val">{{ doomTimer }}</span>
          <span class="clock-lbl">星辰归位之时 (2028-12)</span>
        </div>
      </div>

      <div class="shard shard-bottom-left">
        <div class="glitch-box">
          <h2 class="box-title">灵魂与肉身刻度</h2>
          <div class="stat-grid">
            <div class="stat"><span>理智值 (SAN):</span> <span class="val text-blood">{{ sanityIndex.toFixed(4) }}%</span></div>
            <div class="stat"><span>业障权重:</span> <span class="val">{{ karmaWeight.toFixed(2) }}</span></div>
            <div class="stat"><span>肉身炼金:</span> <span class="val">3-ON / 2-OFF</span></div>
            <div class="stat"><span>灵药剂量:</span> <span class="val" :class="{'hyper-glow': isHyper}">{{ caffeineLvl }} MG</span></div>
          </div>
          <div class="pillar-status mt-4">
            <div class="p-row"><span>Ⅰ 骨 (C语言):</span> <span>{{ pillarC }}%</span></div>
            <div class="p-row"><span>Ⅱ 肉 (数据结构):</span> <span>{{ pillarDS }}%</span></div>
            <div class="p-row"><span>Ⅲ 虚空 (高数):</span> <span>{{ pillarMath }}%</span></div>
          </div>
        </div>
      </div>

      <div class="shard shard-right-mid">
        <h2 class="box-title">阿卡夏记录 (终端日志)</h2>
        <div class="terminal-logs" ref="logContainer">
          <div v-for="(log, idx) in logs" :key="idx" class="log-line" :class="log.type">
            <span class="ts">[{{ log.time }}]</span> {{ log.msg }}
          </div>
        </div>
      </div>

      <div class="shard shard-bottom-center">
        <div class="cli-wrapper">
          <span class="prompt">root@vzuor:仪式#</span>
          <input 
            v-model="cmdInput" 
            @keyup.enter="performRitual"
            class="cli-input" 
            placeholder="献祭你的进度 (例: vocab 50) 或注入灵药 (dose 400)..."
            spellcheck="false" autocomplete="off"
          />
        </div>
      </div>

    </div>

    <div class="death-shroud" v-if="isDead">
      <h1 class="death-words">灵魂剥离... 重置因果律</h1>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, computed, nextTick } from 'vue'

// ============================================================================
// [卷一：真名材质静态导入 (绝对安全机制)]
// 直接 import 解决打包找不到路径和 Canvas 跨域污染的问题
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
// [卷二：阿卡夏状态机 (持久化数据)]
// ============================================================================
const getStore = (key, def) => Number(localStorage.getItem(`v10_${key}`)) || def
const setStore = (key, val) => localStorage.setItem(`v10_${key}`, val)

const vocabSiphoned = ref(getStore('vocab', 2150))
const karmaWeight = ref(getStore('karma', 23.7))
const sanityIndex = ref(getStore('sanity', 99.9998))
const pillarC = ref(getStore('pc', 30))
const pillarDS = ref(getStore('pds', 10))
const pillarMath = ref(getStore('pm', 5))

const caffeineLvl = ref(200)
const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref('')
const isHyper = ref(false)
const isDead = ref(false)
const logs = reactive([])
const logContainer = ref(null)

const vocabPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))
const sanityLevel = computed(() => {
  if (sanityIndex.value > 80) return 'san-safe'
  if (sanityIndex.value > 40) return 'san-warn'
  return 'san-danger'
})

const getTs = () => {
  const d = new Date(); return `${d.getHours()}:${d.getMinutes()}:${d.getSeconds()}`
}

const pushLog = (msg, type = 'info') => {
  logs.push({ time: getTs(), msg, type })
  if (logs.length > 50) logs.shift()
  nextTick(() => { if (logContainer.value) logContainer.value.scrollTop = logContainer.value.scrollHeight })
}

// ============================================================================
// [卷三：混沌祭坛操作系统 (CLI)]
// ============================================================================
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
      sanityIndex.value = Math.min(100, sanityIndex.value + 0.5)
      pushLog(`灵魂已同化。当前祭坛水位: ${vocabPercent.value}%`, 'success')
    } else pushLog('献祭失败：虚无的供品。', 'error')
  } 
  else if (args[0] === 'dose') {
    const dose = parseInt(args[1])
    if (dose > 0) {
      caffeineLvl.value = dose
      if (dose >= 400) {
        isHyper.value = true; window.chaosEngine.hyper = true
        pushLog('警告：灵药过载！血液沸腾，物理法则开始扭曲！', 'error')
        setTimeout(() => { 
          isHyper.value = false; window.chaosEngine.hyper = false; caffeineLvl.value = 200
          pushLog('灵药代谢完成，机体恢复 3-ON 循环。', 'sys')
        }, 8000)
      } else pushLog(`灵药注入。当前剂量 ${dose}MG。`, 'info')
    }
  }
  else if (args[0] === 'vzuor') {
    pushLog('不可名状之真名被唤醒。理智崩塌。', 'error')
    sanityIndex.value -= 15; setStore('sanity', sanityIndex.value)
  }
  else if (args[0] === 'die') {
    isDead.value = true
    setTimeout(() => {
      isDead.value = false
      sanityIndex.value = 100; karmaWeight.value += 10
      setStore('sanity', 100); setStore('karma', karmaWeight.value)
      pushLog('死亡回归完毕。带着新的业障重新开始。', 'sys')
    }, 3000)
  }
  else {
    karmaWeight.value += 0.5; setStore('karma', karmaWeight.value)
    pushLog('无意义的呢喃。业障加深。', 'error')
  }
  cmdInput.value = ''
}

// ============================================================================
// [卷四：2D 神秘学物理模拟引擎 (The Chaos Engine)]
// 包含：坠落的考研知识碎片、漂浮的虚空粒子、反向运动学(IK)触手
// ============================================================================
const voidCanvas = ref(null)
let ctx, w, h, animId, time = 0

// 坠落的知识符文
const arcaneTexts = [
  "C: 指针与内存虚空", "C: 结构体血肉", "DS: 非欧几里得图论", "DS: 虚空二叉树",
  "MATH: 疯狂微积分极限", "MATH: 多重积分吞噬", "22408 飞升矩阵", "KARMA OVERFLOW",
  "Null Pointer Exception", "Segmentation Fault"
]

class FallingDebris {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = Math.random() * this.w; this.y = -50
    this.vx = (Math.random() - 0.5) * 2; this.vy = Math.random() * 1.5 + 0.5
    this.angle = Math.random() * Math.PI; this.va = (Math.random() - 0.5) * 0.05
    this.text = arcaneTexts[Math.floor(Math.random() * arcaneTexts.length)]
    this.size = Math.random() * 10 + 10
  }
  update(hyper) {
    this.x += this.vx * (hyper ? 3 : 1)
    this.y += this.vy * (hyper ? 3 : 1)
    this.angle += this.va * (hyper ? 5 : 1)
    if (this.y > this.h + 50 || this.x < -100 || this.x > this.w + 100) this.reset()
  }
  draw(ctx) {
    ctx.save(); ctx.translate(this.x, this.y); ctx.rotate(this.angle)
    ctx.font = `${this.size}px 'Space Mono', monospace`
    ctx.fillStyle = 'rgba(20, 20, 20, 0.4)'
    ctx.fillText(this.text, 0, 0)
    ctx.restore()
  }
}

// 漂浮的虚空流体粒子
class VoidParticle {
  constructor(w, h) { this.w = w; this.h = h; this.reset() }
  reset() {
    this.x = Math.random() * this.w; this.y = this.h + 50
    this.vx = (Math.random() - 0.5) * 1; this.vy = -(Math.random() * 2 + 0.5)
    this.life = Math.random() * 200 + 100; this.maxLife = this.life
    this.r = Math.random() * 3 + 1
  }
  update(hyper, cx, cy) {
    this.x += this.vx * (hyper ? 2 : 1)
    this.y += this.vy * (hyper ? 2 : 1)
    // 受到中央祭坛的轻微吸引
    const dx = cx - this.x; const dy = cy - this.y; const dist = Math.sqrt(dx*dx+dy*dy)
    if (dist < 400) { this.vx += dx * 0.0005; this.vy += dy * 0.0005 }
    this.life--
    if (this.life < 0) this.reset()
  }
  draw(ctx) {
    ctx.beginPath(); ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(139, 0, 0, ${(this.life/this.maxLife) * 0.6})` // 血液色
    ctx.fill()
  }
}

// 反向运动学(IK)触手追踪
class IKTentacle {
  constructor(x, y, segs, len) {
    this.bx = x; this.by = y; this.segs = segs; this.len = len
    this.joints = Array(segs).fill({x, y}); this.noise = Math.random() * 1000
  }
  reach(tx, ty) {
    this.joints[this.segs-1] = {x: tx, y: ty}
    for(let i=this.segs-2; i>=0; i--) {
      const dx = this.joints[i].x - this.joints[i+1].x; const dy = this.joints[i].y - this.joints[i+1].y
      const ratio = this.len / (Math.sqrt(dx*dx+dy*dy) || 1)
      this.joints[i] = {x: this.joints[i+1].x + dx*ratio, y: this.joints[i+1].y + dy*ratio}
    }
    this.joints[0] = {x: this.bx, y: this.by}
    for(let i=1; i<this.segs; i++) {
      const dx = this.joints[i].x - this.joints[i-1].x; const dy = this.joints[i].y - this.joints[i-1].y
      const ratio = this.len / (Math.sqrt(dx*dx+dy*dy) || 1)
      this.joints[i] = {x: this.joints[i-1].x + dx*ratio, y: this.joints[i-1].y + dy*ratio}
    }
  }
  draw(ctx, time, hyper) {
    ctx.beginPath(); ctx.moveTo(this.joints[0].x, this.joints[0].y)
    for(let i=1; i<this.segs; i++) {
      const wob = Math.sin(time * (hyper?5:2) + i*0.5 + this.noise) * (hyper?10:3)
      const px = this.joints[i].x + wob; const py = this.joints[i].y + wob
      ctx.lineTo(px, py)
    }
    ctx.strokeStyle = hyper ? 'rgba(139, 0, 0, 0.6)' : 'rgba(20, 20, 20, 0.4)'
    ctx.lineWidth = hyper ? 4 : 2; ctx.stroke()
  }
}

const bootPhysicsEngine = () => {
  const cvs = voidCanvas.value; ctx = cvs.getContext('2d')
  w = window.innerWidth; h = window.innerHeight; cvs.width = w; cvs.height = h

  const debris = Array.from({length: 40}, () => new FallingDebris(w, h))
  const particles = Array.from({length: 150}, () => new VoidParticle(w, h))
  const tentacles = Array.from({length: 6}, (_, i) => new IKTentacle(w/2, h/2, 15, 20))

  let mx = w/2, my = h/2
  window.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY })
  window.addEventListener('resize', () => { w = window.innerWidth; h = window.innerHeight; cvs.width = w; cvs.height = h })

  window.chaosEngine = { hyper: false }

  const loop = () => {
    ctx.clearRect(0,0,w,h)
    const cx = w/2, cy = h/2, hyper = window.chaosEngine.hyper

    debris.forEach(d => { d.update(hyper); d.draw(ctx) })
    particles.forEach(p => { p.update(hyper, cx, cy); p.draw(ctx) })
    
    tentacles.forEach((t, i) => {
      // 触手追踪鼠标，部分狂乱游走
      const tx = (i%2===0 && !hyper) ? mx : cx + Math.cos(time + i) * 300
      const ty = (i%2===0 && !hyper) ? my : cy + Math.sin(time*0.8 + i) * 300
      t.reach(tx, ty); t.draw(ctx, time, hyper)
    })

    time += hyper ? 0.05 : 0.015
    animId = requestAnimationFrame(loop)
  }
  loop()
}

// ============================================================================
// [卷五：生命周期与宿命之钟]
// ============================================================================
let clockId
onMounted(() => {
  pushLog('V10.0 终极魔典已装载。光学混合协议启动。', 'sys')
  pushLog('Canvas 物理法则接管完毕。白底已抹杀。', 'sys')
  bootPhysicsEngine()
  
  clockId = setInterval(() => {
    const diff = new Date('2028-12-23T08:30:00') - Date.now()
    const d = Math.floor(diff/86400000).toString().padStart(3,'0')
    const h = Math.floor((diff/3600000)%24).toString().padStart(2,'0')
    const m = Math.floor((diff/60000)%60).toString().padStart(2,'0')
    const s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001)
    setStore('sanity', sanityIndex.value)
  }, 1000)
})

onBeforeUnmount(() => { clearInterval(clockId); cancelAnimationFrame(animId) })
</script>

<style>
/* ============================================================================
   [绝界织物：CSS 光学魔法与失控排版]
   核心：mix-blend-mode: multiply 完美解决 JPG 白底，无需 JS 介入，绝对安全。
   ============================================================================ */
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

:root {
  --c-paper: #E2DED0;
  --c-ink: #111;
  --c-blood: #8B0000;
  --c-muted: #555;
}

body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: var(--c-paper); color: var(--c-ink); font-family: 'Space Mono', monospace; }

.nexus-chaos-root { position: relative; width: 100%; height: 100%; }

/* [0层] 背景 */
.bg-paper-texture { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover; z-index: 0; opacity: 0.9; }
.static-noise-overlay { position: absolute; inset: 0; z-index: 1; pointer-events: none; background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="3"/></filter><rect width="100" height="100" filter="url(%23n)"/></svg>'); opacity: 0.08; }

/* [1层] 光学污染层：绝对消灭白底 */
.optical-corruption { position: absolute; inset: 0; z-index: 5; pointer-events: none; }
.stain { position: absolute; mix-blend-mode: multiply; /* 物理级魔法，白底完全透明，融入羊皮纸 */ filter: contrast(1.2); }
.s-ink1 { top: -10%; left: -5%; width: 45vw; opacity: 0.8; }
.s-ink2 { bottom: -15%; right: -5%; width: 50vw; opacity: 0.9; transform: rotate(15deg); }
.s-ink3 { top: 20%; right: 10%; width: 30vw; opacity: 0.6; transform: rotate(-30deg); }
.s-ink4 { bottom: 10%; left: 10%; width: 40vw; opacity: 0.7; }
.s-ink5 { top: 5%; left: 40%; width: 25vw; opacity: 0.5; }
.s-ten1 { top: 15%; left: 5%; width: 35vw; opacity: 0.25; }
.s-ten2 { bottom: 5%; right: 25%; width: 30vw; opacity: 0.3; }

/* [2层] Canvas 物理引擎 */
.abyssal-physics-canvas { position: absolute; inset: 0; z-index: 10; mix-blend-mode: multiply; pointer-events: none; }

/* [3层] 核心祭坛 */
.altar-core { position: absolute; inset: 0; z-index: 15; display: flex; justify-content: center; align-items: center; pointer-events: none; }
.totem-jesus { height: 75vh; width: auto; mix-blend-mode: multiply; filter: contrast(1.1); transition: transform 0.1s; }
.tremor { animation: violent-shake 0.1s infinite; }

/* 深渊水泡 (53% 的归宿，不再遮挡) */
.void-orb-container { position: absolute; bottom: 10%; display: flex; flex-direction: column; align-items: center; }
.void-orb { width: 100px; height: 100px; border: 2px solid var(--c-ink); border-radius: 50%; background: rgba(226, 222, 208, 0.4); backdrop-filter: blur(4px); display: flex; justify-content: center; align-items: center; overflow: hidden; position: relative; box-shadow: 0 0 20px rgba(0,0,0,0.5); animation: float-orb 4s ease-in-out infinite alternate; pointer-events: auto; }
.water-level { position: absolute; bottom: 0; width: 100%; background: var(--c-ink); transition: height 1s cubic-bezier(0.1, 0.7, 0.1, 1); z-index: 1; }
.orb-data { position: relative; z-index: 2; color: var(--c-blood); font-family: 'Cinzel', serif; text-shadow: 0 0 5px rgba(255,255,255,0.8); }
.orb-data .num { font-size: 2.5rem; font-weight: 900; }
.orb-data .pct { font-size: 1rem; }
.orb-label { margin-top: 10px; font-size: 0.75rem; font-weight: bold; color: var(--c-ink); background: rgba(226, 222, 208, 0.8); padding: 2px 8px; border-radius: 4px; }

/* [4层] 碎片化 UI (绝对定位，避免 Excel 般的规整) */
.shattered-ui-layer { position: absolute; inset: 0; z-index: 20; pointer-events: none; }
.shard { position: absolute; pointer-events: auto; background: rgba(226, 222, 208, 0.6); backdrop-filter: blur(5px); border: 1px dashed rgba(0,0,0,0.3); padding: 1.5rem; box-shadow: 0 0 30px rgba(0,0,0,0.1); }

.shard-top-left { top: 3rem; left: 3rem; background: transparent; border: none; box-shadow: none; padding: 0; }
.chaotic-title { font-family: 'Cinzel', serif; font-size: 3.5rem; font-weight: 900; margin: 0; position: relative; }
.chaotic-title::before { content: attr(data-text); position: absolute; left: -2px; text-shadow: 2px 0 red; opacity: 0.5; animation: glitch-anim 3s infinite linear alternate-reverse; }
.doom-clock { display: flex; flex-direction: column; margin-top: 10px; }
.clock-val { font-size: 2.2rem; font-weight: bold; letter-spacing: -2px; }
.clock-lbl { font-size: 0.8rem; font-weight: bold; color: var(--c-muted); }

.shard-bottom-left { bottom: 4rem; left: 3rem; width: 280px; }
.box-title { font-size: 1rem; border-bottom: 2px solid var(--c-ink); padding-bottom: 5px; margin: 0 0 15px 0; }
.stat-grid { display: flex; flex-direction: column; gap: 8px; font-size: 0.85rem; font-weight: bold; }
.stat { display: flex; justify-content: space-between; }
.text-blood { color: var(--c-blood); }
.hyper-glow { color: red; text-shadow: 0 0 10px red; animation: jitter 0.1s infinite; }
.pillar-status { font-size: 0.75rem; border-top: 1px dashed var(--c-muted); padding-top: 10px; margin-top: 15px; }
.p-row { display: flex; justify-content: space-between; margin-bottom: 5px; }

.shard-right-mid { top: 50%; right: 3rem; transform: translateY(-50%); width: 320px; }
.terminal-logs { height: 250px; overflow-y: auto; font-size: 0.7rem; display: flex; flex-direction: column; gap: 5px; }
.terminal-logs::-webkit-scrollbar { display: none; }
.log-line { border-bottom: 1px solid rgba(0,0,0,0.05); padding-bottom: 3px; }
.log-line .ts { color: var(--c-muted); }
.log-line.sys { color: #555; } .log-line.success { color: green; font-weight: bold; } .log-line.error { color: var(--c-blood); }

.shard-bottom-center { bottom: 2rem; left: 50%; transform: translateX(-50%); width: 600px; background: transparent; border: none; box-shadow: none; }
.cli-wrapper { display: flex; align-items: center; gap: 10px; background: rgba(226, 222, 208, 0.9); padding: 10px 20px; border: 2px solid var(--c-ink); border-radius: 5px; }
.prompt { font-weight: bold; font-size: 1.2rem; color: var(--c-blood); }
.cli-input { flex: 1; border: none; background: transparent; outline: none; font-size: 1.2rem; font-family: inherit; font-style: italic; }
.cli-input::placeholder { color: #888; font-size: 0.9rem; }

/* 死亡回归层 */
.death-shroud { position: fixed; inset: 0; z-index: 9999; background: black; display: flex; justify-content: center; align-items: center; animation: fade-in-out 3s forwards; }
.death-words { color: white; font-size: 4rem; letter-spacing: 10px; animation: violent-shake 0.1s infinite; }

/* 动画库 */
@keyframes jitter { 0% { transform: translateX(1px); } 50% { transform: translateX(-1px); } 100% { transform: translateX(0); } }
@keyframes violent-shake { 0% { transform: translate(2px, 1px) rotate(0deg); } 50% { transform: translate(-1px, 2px) rotate(-1deg); } 100% { transform: translate(1px, -2px) rotate(1deg); } }
@keyframes glitch-anim { 0% { clip: rect(10px, 999px, 30px, 0); } 50% { clip: rect(60px, 999px, 80px, 0); } 100% { clip: rect(20px, 999px, 50px, 0); } }
@keyframes float-orb { 0% { transform: translateY(0px); } 100% { transform: translateY(-15px); } }
@keyframes fade-in-out { 0% { opacity: 1; } 80% { opacity: 1; } 100% { opacity: 0; } }

/* 理智滤镜绑定 */
.san-warn { filter: contrast(1.1) sepia(0.2); }
.san-danger { filter: contrast(1.3) sepia(0.4) hue-rotate(-10deg); }
</style>