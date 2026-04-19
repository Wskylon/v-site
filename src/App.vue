/* [阿卡姆飞升魔典 V8.0 // 绝对失控版] */
<template>
  <div id="vzuor-nexus" class="nexus-root">
    
    <canvas ref="mainCanvas" class="abyssal-canvas"></canvas>

    <div class="corruption-layer">
      <div class="ink-stain ink-1" :style="{ backgroundImage: `url(${getImg('ink1.jpg')})` }"></div>
      <div class="ink-stain ink-2" :style="{ backgroundImage: `url(${getImg('ink2.jpg')})` }"></div>
      <div class="ink-stain ink-3" :style="{ backgroundImage: `url(${getImg('ink3.jpg')})` }"></div>
      <div class="ink-stain ink-4" :style="{ backgroundImage: `url(${getImg('ink4.jpg')})` }"></div>
      <div class="ink-stain ink-5" :style="{ backgroundImage: `url(${getImg('ink5.jpg')})` }"></div>
      <div class="ink-stain tentacle-1" :style="{ backgroundImage: `url(${getImg('tentacle1.jpg')})` }"></div>
      <div class="ink-stain tentacle-2" :style="{ backgroundImage: `url(${getImg('tentacle2.jpg')})` }"></div>
    </div>

    <div class="shattered-ui">
      
      <header class="chaos-header">
        <h1 class="arcane-title">V'ZUOR KHAA-SH'AN</h1>
        <div class="doom-timer">{{ doomTimer }}</div>
      </header>

      <main class="altar-center">
        <div class="totem-container">
          <img :src="getImg('totem.jpg')" class="totem-image" />
          
          <div class="siphon-pool">
            <div class="pool-water" :style="{ height: vocabPercent + '%' }"></div>
            <div class="pool-glass"></div>
          </div>
          
          <div class="pool-readout">
            <span class="vocab-number">{{ vocabSiphoned }}</span> / 4000
          </div>
        </div>
      </main>

      <aside class="sidebar left-sidebar">
        <div class="data-block">
          <h3>THE_PILLARS</h3>
          <div class="mini-row">FLESH (C): <span>ASSIMILATING</span></div>
          <div class="mini-row">BONE (DS): <span>RECONSTRUCTING</span></div>
          <div class="mini-row">VOID (MATH): <span>COMPREHENDING</span></div>
        </div>
        <div class="data-block mt">
          <h3>FLESH_ALCHEMY</h3>
          <div class="mini-row">CYCLE: <span>3-ON / 2-OFF</span></div>
          <div class="mini-row">DOSE: <span class="blood-text">{{ caffeine }} MG</span></div>
        </div>
      </aside>

      <aside class="sidebar right-sidebar">
        <div class="data-block">
          <h3>BIOMETRICS</h3>
          <div class="mini-row">SANITY: <span class="blood-text">{{ sanityIndex.toFixed(4) }}%</span></div>
          <div class="mini-row">KARMA: <span>{{ karmaLoad.toFixed(2) }}</span></div>
        </div>
        <div class="log-container">
          <div v-for="(log, i) in logs" :key="i" class="log-line">[{{ log.ts }}] {{ log.msg }}</div>
        </div>
      </aside>

      <footer class="chaos-footer">
        <div class="cli-wrap">
          <span class="prompt">root@vzuor:#</span>
          <input v-model="cmdInput" @keyup.enter="handleCmd" placeholder="AWAITING_SACRIFICE..." />
        </div>
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed, reactive, nextTick } from 'vue'

const getImg = (name) => new URL(`./assets/${name}`, import.meta.url).href

const vocabSiphoned = ref(Number(localStorage.getItem('v8_vocab')) || 2150)
const sanityIndex = ref(99.9999)
const karmaLoad = ref(23.70)
const caffeine = ref(200)
const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref('')
const logs = reactive([])

const vocabPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

const mainCanvas = ref(null)
let ctx, w, h, time = 0

// 抹除白底的底层算法
const processImgData = (imgSrc) => {
  return new Promise((resolve) => {
    const img = new Image()
    img.src = imgSrc
    img.onload = () => {
      const c = document.createElement('canvas'); const t = c.getContext('2d')
      c.width = img.width; c.height = img.height
      t.drawImage(img, 0, 0)
      const id = t.getImageData(0, 0, c.width, c.height); const d = id.data
      for (let i = 0; i < d.length; i += 4) {
        if (d[i] + d[i+1] + d[i+2] > 700) d[i+3] = 0
      }
      t.putImageData(id, 0, 0)
      resolve(c)
    }
  })
}

const initCanvas = async () => {
  const cvs = mainCanvas.value
  ctx = cvs.getContext('2d')
  w = window.innerWidth; h = window.innerHeight
  cvs.width = w; cvs.height = h

  const paper = await new Promise(r => { const i = new Image(); i.src = getImg('paper.jpg'); i.onload = () => r(i) })

  const render = () => {
    ctx.clearRect(0,0,w,h)
    ctx.drawImage(paper, 0, 0, w, h)
    time += 0.05
    requestAnimationFrame(render)
  }
  render()
}

onMounted(() => {
  initCanvas()
  setInterval(() => {
    const diff = new Date('2028-12-23T08:30:00') - Date.now()
    const d = Math.floor(diff/86400000).toString().padStart(3,'0')
    const h = Math.floor((diff/3600000)%24).toString().padStart(2,'0')
    const m = Math.floor((diff/60000)%60).toString().padStart(2,'0')
    const s = Math.floor((diff/1000)%60).toString().padStart(2,'0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
  }, 1000)
  logs.push({ ts: new Date().toLocaleTimeString(), msg: 'V8.0: 规整已被粉碎。' })
})

const handleCmd = () => {
  const val = parseInt(cmdInput.value.split(' ')[1])
  if (cmdInput.value.startsWith('vocab') && !isNaN(val)) {
    vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + val)
    localStorage.setItem('v8_vocab', vocabSiphoned.value)
    logs.push({ ts: new Date().toLocaleTimeString(), msg: `祭坛水位上升：+${val}` })
  }
  cmdInput.value = ''
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap');

.nexus-root { position: relative; width: 100vw; height: 100vh; overflow: hidden; font-family: 'Space Mono', monospace; background: #E2DED0; color: #111; }
.abyssal-canvas { position: absolute; inset: 0; z-index: 1; }

/* 污染层：彻底释放 JPG 素材 */
.corruption-layer { position: absolute; inset: 0; z-index: 5; pointer-events: none; overflow: hidden; }
.ink-stain { position: absolute; background-size: contain; background-repeat: no-repeat; mix-blend-mode: multiply; filter: contrast(1.2); }
.ink-1 { top: -5%; left: -5%; width: 500px; height: 500px; opacity: 0.8; }
.ink-2 { bottom: -10%; right: -5%; width: 600px; height: 600px; opacity: 0.9; transform: rotate(15deg); }
.ink-3 { top: 20%; right: -10%; width: 400px; height: 400px; opacity: 0.7; transform: rotate(-45deg); }
.ink-4 { bottom: 10%; left: -15%; width: 500px; height: 500px; opacity: 0.6; }
.ink-5 { top: -10%; right: 20%; width: 300px; height: 300px; opacity: 0.5; }
.tentacle-1 { top: 30%; left: 5%; width: 400px; height: 400px; opacity: 0.2; }
.tentacle-2 { bottom: 5%; right: 20%; width: 350px; height: 350px; opacity: 0.3; }

/* UI 层 */
.shattered-ui { position: relative; z-index: 10; height: 100%; display: flex; flex-direction: column; pointer-events: none; }
.shattered-ui * { pointer-events: auto; }

.chaos-header { display: flex; justify-content: space-between; padding: 2rem 4rem; }
.arcane-title { font-family: 'Cinzel', serif; font-size: 3rem; margin: 0; font-weight: 900; letter-spacing: 2px; }
.doom-timer { font-size: 2rem; font-weight: bold; letter-spacing: -1px; }

/* 核心祭坛区 */
.altar-center { flex: 1; display: flex; justify-content: center; align-items: center; position: relative; }
.totem-container { position: relative; width: 500px; height: 700px; display: flex; justify-content: center; align-items: center; }
.totem-image { position: absolute; width: 100%; height: 100%; object-fit: contain; mix-blend-mode: multiply; z-index: 15; }

/* 黑水进度池 */
.siphon-pool { position: absolute; bottom: 20%; width: 180px; height: 250px; z-index: 20; display: flex; align-items: flex-end; justify-content: center; }
.pool-glass { position: absolute; inset: 0; border: 2px solid rgba(0,0,0,0.8); border-radius: 10px 10px 30px 30px; background: rgba(255,255,255,0.1); backdrop-filter: blur(2px); box-shadow: 0 0 20px rgba(0,0,0,0.5); }
.pool-water { width: 100%; background: #0A0A0A; border-radius: 0 0 25px 25px; transition: height 1s ease; box-shadow: inset 0 10px 20px rgba(0,0,0,0.8); }
.pool-readout { position: absolute; bottom: 15%; z-index: 25; font-family: 'Cinzel', serif; font-size: 1.2rem; color: #E2DED0; text-shadow: 0 0 5px black; font-weight: bold; }
.vocab-number { font-size: 2rem; color: #8B0000; }

/* 侧边栏：推向边缘 */
.sidebar { position: absolute; top: 50%; transform: translateY(-50%); width: 250px; background: rgba(226, 222, 208, 0.7); padding: 1.5rem; border: 1px dashed rgba(0,0,0,0.3); backdrop-filter: blur(4px); box-shadow: 0 0 30px rgba(0,0,0,0.1); }
.left-sidebar { left: 3rem; }
.right-sidebar { right: 3rem; }

.data-block h3 { font-size: 0.9rem; border-bottom: 1px solid rgba(0,0,0,0.8); padding-bottom: 5px; margin-bottom: 15px; letter-spacing: 1px; }
.mini-row { display: flex; justify-content: space-between; font-size: 0.75rem; margin-bottom: 8px; font-weight: bold; }
.mt { margin-top: 2rem; }
.blood-text { color: #8B0000; font-weight: 900; }

.log-container { height: 150px; overflow-y: auto; font-size: 0.65rem; border: 1px solid rgba(0,0,0,0.2); padding: 10px; background: rgba(0,0,0,0.05); margin-top: 2rem; }
.log-line { margin-bottom: 5px; color: #333; }

.chaos-footer { padding: 2rem 4rem; border-top: 1px solid rgba(0,0,0,0.2); }
.cli-wrap { display: flex; align-items: center; gap: 1rem; }
.prompt { font-weight: bold; font-size: 1.2rem; color: #0A0A0A; }
input { flex: 1; background: transparent; border: none; border-bottom: 1px dashed rgba(0,0,0,0.5); font-size: 1.5rem; font-family: inherit; font-style: italic; outline: none; padding-bottom: 5px; }
input:focus { border-bottom: 1px solid #8B0000; }
</style>