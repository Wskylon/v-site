<template>
  <div id="vzuor-absolute-frame" :class="{ 'screen-shake': isGlitching }" @mousemove="handleGlobalMouse">
    
    <div id="native-cursor-container">
      <div id="native-core" class="custom-cursor-core"></div>
    </div>

    <div v-if="isGlitching" class="eldritch-terror-overlay">
      <canvas ref="swarmCanvasRef" class="swarm-canvas-layer"></canvas>
      <div class="terror-eye">
        <svg viewBox="0 0 200 120">
          <path d="M 10 60 C 40 -10, 160 -10, 190 60 C 160 130, 40 130, 10 60 Z" fill="#150000" stroke="#ff0033" stroke-width="3"/>
          <ellipse cx="100" cy="60" rx="25" ry="45" fill="#ff0000" class="terror-pupil"/>
          <ellipse cx="100" cy="60" :rx="2" :ry="48 + pupilPulseY" fill="#000"/>
          <path d="M 50 30 L 30 15 M 150 30 L 170 15 M 40 90 L 20 105 M 160 90 L 180 105 M 100 105 L 95 140 M 120 100 L 140 120 M 80 100 L 60 120" stroke="#ff0033" stroke-width="2" class="blood-veins"/>
        </svg>
      </div>
      <div class="terror-vignette"></div>
    </div>

    <div class="liquid-distort-container" :class="[activeFateTheme, { 'is-tearing-chromatic': isBoardOpen || isDestinyReadingOpen }]">
      <div class="wing left-wing"><img :src="imgZuo" class="inverted liquid-image" /></div>
      <div class="center-main" ref="centerContainer">
        <img :src="imgZhong" class="liquid-image" />
      </div>
      <div class="wing right-wing"><img :src="imgYou" class="inverted liquid-image" /></div>
    </div>

    <div class="left-ui-layer">
      <div class="flesh-scar-btn" :style="{ top: palmY + '%', left: palmX + '%' }" @click="triggerSpaceTear">
        <svg viewBox="0 0 100 100" class="abyssal-sigil">
          <path d="M 10 50 Q 50 -10 90 50 Q 50 110 10 50" fill="rgba(42, 92, 77, 0.3)" filter="blur(2px)"/>
          <path d="M 20 50 L 50 20 L 80 50 L 50 80 Z" fill="none" stroke="#998550" stroke-width="1.5" />
          <circle cx="50" cy="50" r="4" fill="#ff0033" class="sigil-glow" />
        </svg>
        <span class="sigil-text">启示</span>
      </div>
    </div>

    <div class="center-ui-layer">
      <div class="eye-deep-core" :style="{ top: eyeY + '%', left: eyeX + '%', width: eyeSize + 'vw' }">
        <div class="void-ring"></div>
        <div class="pupil-nebula" :style="{ transform: `translate(calc(-50% + ${pupilOffset.x}px), calc(-50% + ${pupilOffset.y}px))` }">
          <div class="pupil-core-vertical" :style="{ height: 60 + pupilPulseBase * 10 + '%' }"></div>
        </div>
      </div>

      <div class="void-crystal-btn" 
           :class="{ 'is-freaking-out': crystalHighlight }"
           :style="{ top: crystalY + '%', left: crystalX + '%' }" 
           @click="openCoreRitual">
        <svg viewBox="0 0 100 150" class="crystal-svg">
          <path d="M50 0 L80 40 Q95 80 60 120 L50 150 L40 120 Q5 80 20 40 Z" fill="rgba(10, 20, 15, 0.8)" stroke="#2a5c4d" stroke-width="2" class="crystal-shell"/>
          <path d="M50 20 L70 50 Q75 80 55 110 L50 130 L45 110 Q25 80 30 50 Z" fill="none" stroke="#998550" stroke-width="1.5" class="crystal-veins"/>
          <circle cx="50" cy="70" r="8" fill="#998550" class="crystal-eye"/>
        </svg>
        <span class="sigil-text" :class="{'glitch-label': crystalHighlight}">核心锚定</span>
      </div>
    </div>

    <div class="right-ui-layer">
      <div class="destiny-circle-btn" 
           :class="{ 'is-freaking-out': circleHighlight }"
           :style="{ top: circleY + '%', left: circleX + '%' }" 
           @click="openThreeCardRitual">
        <svg viewBox="0 0 100 100" class="magic-circle-svg">
          <circle cx="50" cy="50" r="45" fill="none" stroke="#2a5c4d" stroke-width="2" stroke-dasharray="8 4" class="spin-slow"/>
          <circle cx="50" cy="50" r="35" fill="none" stroke="#998550" stroke-width="1.5" class="spin-fast-reverse"/>
          <polygon points="50,15 85,75 15,75" fill="none" stroke="#ff0033" stroke-width="1" class="pulse-alpha"/>
          <polygon points="50,85 15,25 85,25" fill="none" stroke="#ff0033" stroke-width="1" class="pulse-alpha"/>
          <circle cx="50" cy="50" r="10" fill="#998550" class="circle-core-eye"/>
        </svg>
        <span class="sigil-text" :class="{'glitch-label': circleHighlight}">命运指引</span>
      </div>
    </div>

    <transition name="space-rip" @after-enter="cleanUpTearFilter">
      <div v-if="isBoardOpen" class="message-board-overlay" :class="{ 'is-tearing': isTearing }">
        <div class="cthulhu-modal">
          <button class="close-btn" @click="closeSpaceTear">×</button>
          <div class="modal-header">
            <h2 class="ancient-title">启 示 录</h2>
            <div class="eldritch-divider"></div>
          </div>
          <div class="modal-content">
            <div class="messages-list-wrapper">
              <div class="messages-list" ref="msgListRef">
                <transition-group name="msg-fade">
                  <div v-for="msg in messages" :key="msg.id" class="msg-item">
                    <span class="fate-icon" v-html="msg.icon"></span>
                    <span class="msg-author">[{{ msg.author }}]</span>
                    <span class="msg-text">{{ msg.displayText }}</span>
                  </div>
                </transition-group>
                <div v-if="messages.length === 0" class="empty-state">... 虚 空 寂 静 ...</div>
              </div>
            </div>
            <div class="input-area">
              <textarea v-model="newMessage" @keyup.enter="submitMessage" placeholder="在此刻下呓语..." class="abyssal-textarea"></textarea>
              <button class="submit-btn" @click="submitMessage">献 祭</button>
            </div>
            <div class="anonymous-notice">这是一个匿名留言板，欢迎大家留言</div>
          </div>
        </div>
      </div>
    </transition>

    <transition name="void-dissolve">
      <div v-if="isCoreRitualOpen" class="tarot-ritual-overlay">
        <div class="ritual-stage">
          <button class="close-btn" @click="closeCoreRitual" style="top:-30px; right:-30px;">×</button>
          <h3 class="ritual-title">{{ hasCoreRevealed ? '核 心 已 定' : '确 立 锚 点' }}</h3>
          
          <div class="tarot-card-3d" :class="{ 'flipped': hasCoreRevealed }" @click="revealCoreCard">
            <div class="card-face card-back"><div class="back-pattern"></div></div>
            <div class="card-face card-front">
              <div v-if="hasCoreRevealed" class="card-svg-wrap" :class="{'reversed-svg': coreCardData.isReversed}" v-html="coreCardData.svg"></div>
              <div v-if="hasCoreRevealed" class="card-name">
                {{ coreCardData.name }} <span class="card-state">{{ coreCardData.isReversed ? '(逆位)' : '(正位)' }}</span>
              </div>
              <div v-if="hasCoreRevealed" class="card-desc">{{ coreCardData.desc }}</div>
            </div>
          </div>

          <div v-if="hasCoreRevealed" class="ritual-hint">核心锚点已确立，请前往右翼法阵展开宿命</div>
          <div v-else class="ritual-hint blink-hint">点击抽取您的核心锚点</div>
        </div>
      </div>
    </transition>

    <transition name="void-dissolve">
      <div v-if="isThreeCardRitualOpen" class="tarot-ritual-overlay">
        <div class="ritual-stage three-cards-stage">
          <button class="close-btn" @click="closeThreeCardRitual" style="top:-30px; right:20px;">×</button>
          <h3 class="ritual-title">展 开 宿 命</h3>
          
          <div class="cards-layout">
            <div class="card-slot">
              <div class="slot-title">【左位·溯源】</div>
              <div class="tarot-card-3d" :class="{ 'flipped': hasLeftRevealed }" @click="revealLeftCard">
                <div class="card-face card-back"><div class="back-pattern"></div></div>
                <div class="card-face card-front">
                  <div v-if="hasLeftRevealed" class="card-svg-wrap" :class="{'reversed-svg': leftCardData.isReversed}" v-html="leftCardData.svg"></div>
                  <div v-if="hasLeftRevealed" class="card-name">
                    {{ leftCardData.name }} <span class="card-state">{{ leftCardData.isReversed ? '(逆位)' : '(正位)' }}</span>
                  </div>
                  <div v-if="hasLeftRevealed" class="card-desc">过往的成因与羁绊</div>
                </div>
              </div>
            </div>

            <div class="card-slot core-slot">
              <div class="slot-title core-title">【中位·核心】</div>
              <div class="tarot-card-3d" :class="{ 'flipped': hasCoreRevealed }" @click="warnCoreUnflipped">
                <div class="card-face card-back"><div class="back-pattern"></div></div>
                <div class="card-face card-front">
                  <div v-if="hasCoreRevealed" class="card-svg-wrap" :class="{'reversed-svg': coreCardData.isReversed}" v-html="coreCardData.svg"></div>
                  <div v-if="hasCoreRevealed" class="card-name">
                    {{ coreCardData.name }} <span class="card-state">{{ coreCardData.isReversed ? '(逆位)' : '(正位)' }}</span>
                  </div>
                  <div v-if="hasCoreRevealed" class="card-desc">{{ coreCardData.desc }}</div>
                </div>
              </div>
            </div>

            <div class="card-slot">
              <div class="slot-title">【右位·指引】</div>
              <div class="tarot-card-3d" :class="{ 'flipped': hasRightRevealed }" @click="revealRightCard">
                <div class="card-face card-back"><div class="back-pattern"></div></div>
                <div class="card-face card-front">
                  <div v-if="hasRightRevealed" class="card-svg-wrap" :class="{'reversed-svg': rightCardData.isReversed}" v-html="rightCardData.svg"></div>
                  <div v-if="hasRightRevealed" class="card-name">
                    {{ rightCardData.name }} <span class="card-state">{{ rightCardData.isReversed ? '(逆位)' : '(正位)' }}</span>
                  </div>
                  <div v-if="hasRightRevealed" class="card-desc">未来的宿命与突破</div>
                </div>
              </div>
            </div>
          </div>

          <transition name="fade-sys">
            <div v-if="warningMsg" class="validation-msg">{{ warningMsg }}</div>
          </transition>
          <div v-if="!warningMsg && !allRevealed" class="ritual-hint">必须先在中央晶石确立核心，方可翻开溯源与指引</div>
          
          <button v-if="allRevealed" class="accept-btn" @click="openDestinyReading">直视命运指引</button>
        </div>
      </div>
    </transition>

    <transition name="space-rip">
      <div v-if="isDestinyReadingOpen" class="destiny-reading-overlay">
        <div class="flesh-modal">
          <div class="flesh-modal-bg-texture"></div>
          <button class="close-btn mad-close" @click="isDestinyReadingOpen = false">×</button>
          
          <div class="madness-header">
            <h2 class="ancient-title twisted-text" data-text="命 运 指 引">命 运 指 引</h2>
          </div>
          
          <div class="madness-content">
            
            <div class="flesh-section">
              <h4 class="mad-section-title">
                【中位 · 核心锚定】 
                <span class="chromatic-bleed">{{ coreCardData.name }} {{ coreCardData.isReversed ? '(逆位)' : '(正位)' }}</span>
              </h4>
              <div class="mad-text" v-html="generateReadingText('core', coreCardData)"></div>
            </div>
            
            <div class="flesh-section">
              <h4 class="mad-section-title">
                【左位 · 宿命溯源】 
                <span class="chromatic-bleed">{{ leftCardData.name }} {{ leftCardData.isReversed ? '(逆位)' : '(正位)' }}</span>
              </h4>
              <div class="mad-text" v-html="generateReadingText('left', leftCardData)"></div>
            </div>

            <div class="flesh-section">
              <h4 class="mad-section-title">
                【右位 · 破局指引】 
                <span class="chromatic-bleed">{{ rightCardData.name }} {{ rightCardData.isReversed ? '(逆位)' : '(正位)' }}</span>
              </h4>
              <div class="mad-text" v-html="generateReadingText('right', rightCardData)"></div>
            </div>

            <div class="flesh-section final-void">
              <h4 class="mad-section-title" style="justify-content: center; text-shadow: 0 0 15px red;">【虚空忠告】</h4>
              <div class="mad-text text-center">
                <p>能让你掌控命运的，从来不是逃避与抗拒，而是看清核心的真相。</p>
                <p>深渊的契约已结成，时空的闭环已展露，接下来，全凭你的意志。</p>
              </div>
            </div>

          </div>
        </div>
      </div>
    </transition>

    <div class="blind-spot-trigger" @click="toggleDevPanel"></div>
    <transition name="fade-sys">
      <div v-if="isDevPanelOpen" class="dev-terminal">
        <div class="terminal-header"><span>// CREATOR_VOID</span><button @click="toggleDevPanel" class="sys-close">X</button></div>
        <div class="sys-group">眼 X/Y/S: <input v-model.number="eyeX" type="range"><input v-model.number="eyeY" type="range"><input v-model.number="eyeSize" type="range"></div>
        <div class="sys-group">左印记 X/Y: <input v-model.number="palmX" type="range"><input v-model.number="palmY" type="range"></div>
        <div class="sys-group">中晶石 X/Y: <input v-model.number="crystalX" type="range"><input v-model.number="crystalY" type="range"></div>
        <div class="sys-group">右法阵 X/Y: <input v-model.number="circleX" type="range"><input v-model.number="circleY" type="range"></div>
        <button class="reset-sys-btn" @click="resetCoordinates">RESET</button>
      </div>
    </transition>

    <svg width="0" height="0" style="position:absolute;">
      <filter id="full-liquid-breathing" x="-30%" y="-30%" width="160%" height="160%">
        <feTurbulence type="fractalNoise" baseFrequency="0.005" numOctaves="3" result="noise">
          <animate attributeName="baseFrequency" values="0.005; 0.008; 0.005" dur="15s" repeatCount="indefinite"/>
        </feTurbulence>
        <feDisplacementMap in="SourceGraphic" in2="noise" scale="40" xChannelSelector="R" yChannelSelector="G"/>
      </filter>
      <filter id="space-tear-filter" x="-50%" y="-50%" width="200%" height="200%">
        <feTurbulence type="fractalNoise" baseFrequency="0.05" numOctaves="3" result="ripNoise">
          <animate attributeName="baseFrequency" values="0.05; 0.001" dur="1s" fill="freeze" />
        </feTurbulence>
        <feDisplacementMap in="SourceGraphic" in2="ripNoise" scale="120" xChannelSelector="R" yChannelSelector="G" id="tear-displacement">
          <animate attributeName="scale" values="120; 0" dur="1s" fill="freeze" />
        </feDisplacementMap>
      </filter>
      <filter id="text-madness" x="-20%" y="-20%" width="140%" height="140%">
        <feTurbulence type="fractalNoise" baseFrequency="0.02 0.1" numOctaves="2" result="warp">
          <animate attributeName="baseFrequency" values="0.02 0.1; 0.01 0.05; 0.02 0.1" dur="5s" repeatCount="indefinite"/>
        </feTurbulence>
        <feDisplacementMap xChannelSelector="R" yChannelSelector="G" scale="10" in="SourceGraphic" in2="warp"/>
      </filter>
    </svg>
  </div>
</template>

<script setup>
import { ref, reactive, nextTick, onMounted, watch, computed } from 'vue';
import { tarotLore } from './tarot-lore.js'; // 🌟 引入外部深渊设定库 🌟
import imgZhong from './assets/zhong.jpg'; 
import imgZuo from './assets/zuo1.jpg'; 
import imgYou from './assets/you2.jpg';

// ==========================================
// 🔒 安全坐标系统
// ==========================================
const defaultValues = { 
  eyeX: 50.6, eyeY: 9.4, eyeSize: 7.8, 
  palmX: 50.0, palmY: 50.0, 
  crystalX: 50.0, crystalY: 85.0,
  circleX: 50.0, circleY: 50.0 
};
const eyeX = ref(defaultValues.eyeX), eyeY = ref(defaultValues.eyeY), eyeSize = ref(defaultValues.eyeSize);
const palmX = ref(defaultValues.palmX), palmY = ref(defaultValues.palmY);
const crystalX = ref(defaultValues.crystalX), crystalY = ref(defaultValues.crystalY);
const circleX = ref(defaultValues.circleX), circleY = ref(defaultValues.circleY);

const getSafeStorage = (key, defaultVal) => {
  const val = localStorage.getItem('void_' + key);
  if (val !== null) { const parsed = parseFloat(val); if (!isNaN(parsed)) return parsed; }
  return defaultVal;
};

onMounted(() => {
  eyeX.value = getSafeStorage('eyeX', defaultValues.eyeX);
  eyeY.value = getSafeStorage('eyeY', defaultValues.eyeY);
  eyeSize.value = getSafeStorage('eyeSize', defaultValues.eyeSize);
  palmX.value = getSafeStorage('palmX', defaultValues.palmX);
  palmY.value = getSafeStorage('palmY', defaultValues.palmY);
  crystalX.value = getSafeStorage('crystalX', defaultValues.crystalX);
  crystalY.value = getSafeStorage('crystalY', defaultValues.crystalY);
  circleX.value = getSafeStorage('circleX', defaultValues.circleX);
  circleY.value = getSafeStorage('circleY', defaultValues.circleY);
  
  initNativeEngine(); 
});

watch([eyeX, eyeY, eyeSize, palmX, palmY, crystalX, crystalY, circleX, circleY], (vals) => {
  ['eyeX','eyeY','eyeSize','palmX','palmY','crystalX','crystalY','circleX','circleY'].forEach((k,i) => localStorage.setItem('void_'+k, vals[i]));
});

const resetCoordinates = () => { 
  eyeX.value = 50.6; eyeY.value = 9.4; eyeSize.value = 7.8; 
  palmX.value = 50.0; palmY.value = 50.0; 
  crystalX.value = 50.0; crystalY.value = 85.0; 
  circleX.value = 50.0; circleY.value = 50.0;
};

// ==========================================
// 🌟 零延迟原生光标引擎 🌟
// ==========================================
let nativeTrails = [];
let cursorHistory = Array(20).fill({x: -100, y: -100});
let globalMouse = { x: -100, y: -100 };
const pupilPulseBase = ref(1);
const pupilPulseY = ref(0);
const pupilOffset = reactive({ x: 0, y: 0 });
let lastMouse = { x: 0, y: 0 };
let currentSpeed = 0;
const centerContainer = ref(null);

const initNativeEngine = () => {
  const container = document.getElementById('native-cursor-container');
  if (!container) return;
  for (let i = 0; i < 20; i++) {
    let t = document.createElement('div');
    t.className = 'native-cursor-trail';
    container.appendChild(t);
    nativeTrails.push(t);
  }
  window.addEventListener('mousemove', (e) => {
    globalMouse.x = e.clientX; globalMouse.y = e.clientY;
  }, { passive: true });
  requestAnimationFrame(nativeRenderLoop);
};

setInterval(() => { pupilPulseBase.value = 0.8 + Math.random() * 0.4; }, 100);

const nativeRenderLoop = () => {
  cursorHistory.unshift({ x: globalMouse.x, y: globalMouse.y });
  cursorHistory.pop();

  const core = document.getElementById('native-core');
  if(core) core.style.transform = `translate3d(${globalMouse.x}px, ${globalMouse.y}px, 0)`;

  nativeTrails.forEach((trail, i) => {
    const pos = cursorHistory[i];
    const scale = 1 - (i / 20); 
    trail.style.transform = `translate3d(${pos.x}px, ${pos.y}px, 0) scale(${scale})`;
    trail.style.opacity = scale;
  });

  let rawSpeed = Math.abs(globalMouse.x - lastMouse.x) + Math.abs(globalMouse.y - lastMouse.y);
  currentSpeed += (rawSpeed - currentSpeed) * 0.1; 
  pupilPulseY.value += (Math.min(45, currentSpeed * 0.5) - pupilPulseY.value) * 0.1; 
  lastMouse.x = globalMouse.x; lastMouse.y = globalMouse.y;

  if (centerContainer.value) {
    const r = centerContainer.value.getBoundingClientRect();
    const cX = r.left + (r.width * eyeX.value / 100), cY = r.top + (r.height * eyeY.value / 100);
    const max = (window.innerWidth * eyeSize.value / 100) * 0.15;
    const dX = globalMouse.x - cX, dY = globalMouse.y - cY;
    const dist = Math.sqrt(dX*dX + dY*dY);
    if (dist > max) { 
      const a = Math.atan2(dY, dX); pupilOffset.x = max*Math.cos(a); pupilOffset.y = max*Math.sin(a); 
    } else { 
      pupilOffset.x = dX; pupilOffset.y = dY; 
    }
  }
  requestAnimationFrame(nativeRenderLoop);
};

// ==========================================
// 🚨 生物簇群 万瞳 Canvas 渲染引擎 🚨
// ==========================================
const swarmCanvasRef = ref(null);
let swarmAnimId = null;

const startCanvasSwarm = () => {
  const cvs = swarmCanvasRef.value;
  if (!cvs) return;
  cvs.width = window.innerWidth; cvs.height = window.innerHeight;
  const ctx = cvs.getContext('2d');

  const clusters = Array.from({length: 30}, () => ({ x: Math.random() * cvs.width, y: Math.random() * cvs.height }));
  const eyes = Array.from({ length: 1500 }, () => {
    const c = clusters[Math.floor(Math.random() * clusters.length)];
    const r1 = Math.random(), r2 = Math.random();
    const radius = Math.sqrt(-2.0 * Math.log(r1)) * Math.cos(2.0 * Math.PI * r2) * 120;
    const angle = Math.random() * Math.PI * 2;
    return {
      x: c.x + radius * Math.cos(angle), y: c.y + radius * Math.sin(angle),
      s: Math.random() * 8 + 4, r: Math.random() * Math.PI * 2, 
      blinkPhase: Math.random() * Math.PI * 2, blinkSpeed: Math.random() * 0.08 + 0.02, 
      isGold: Math.random() > 0.85
    };
  });

  const draw = () => {
    ctx.clearRect(0, 0, cvs.width, cvs.height);
    eyes.forEach(eye => {
      eye.blinkPhase += eye.blinkSpeed;
      let openRatio = (Math.sin(eye.blinkPhase) + 1) / 2;
      if (openRatio < 0.1) openRatio = 0.1;

      ctx.save();
      ctx.translate(eye.x, eye.y); ctx.rotate(eye.r); ctx.scale(1, openRatio); 
      ctx.fillStyle = '#050000';
      ctx.beginPath(); ctx.ellipse(0, 0, eye.s, eye.s * 0.6, 0, 0, Math.PI * 2); ctx.fill();
      ctx.strokeStyle = '#ff0033'; ctx.lineWidth = 1; ctx.stroke();
      ctx.fillStyle = eye.isGold ? '#ffd700' : '#ff0033';
      ctx.beginPath(); ctx.arc(0, 0, eye.s * 0.4, 0, Math.PI * 2); ctx.fill();
      ctx.fillStyle = '#000';
      ctx.beginPath(); ctx.ellipse(0, 0, eye.s * 0.1, eye.s * 0.35, 0, 0, Math.PI * 2); ctx.fill();
      ctx.restore();
    });
    swarmAnimId = requestAnimationFrame(draw);
  };
  draw();
};

const stopCanvasSwarm = () => {
  if (swarmAnimId) { cancelAnimationFrame(swarmAnimId); swarmAnimId = null; }
};

// ==========================================
// 🃏 数据引擎：融合 tarot-lore.js 与 独立SVG
// ==========================================
const visualExtras = [
  { icon: "✨", theme: "fate-fool", svg: `<svg viewBox="0 0 100 150"><circle cx="50" cy="75" r="30" stroke="#ffd700" fill="none" stroke-width="2"/><path d="M50 20 V130" stroke="#ff0033" stroke-width="2"/></svg>` },
  { icon: "🌌", theme: "fate-magician", svg: `<svg viewBox="0 0 100 150"><path d="M20 130 L80 130 L50 20 Z" stroke="#2a5c4d" stroke-width="2" fill="none"/></svg>` },
  { icon: "🌙", theme: "fate-priestess", svg: `<svg viewBox="0 0 100 150"><rect x="30" y="30" width="40" height="90" stroke="#507a66" stroke-width="2" fill="none"/></svg>` },
  { icon: "🩸", theme: "fate-empress", svg: `<svg viewBox="0 0 100 150"><path d="M50 20 C80 50 80 100 50 130 C20 100 20 50 50 20" stroke="#ff0033" stroke-width="2" fill="none"/></svg>` },
  { icon: "⚖️", theme: "fate-emperor", svg: `<svg viewBox="0 0 100 150"><rect x="20" y="20" width="60" height="110" stroke="#998550" stroke-width="2" fill="none"/></svg>` },
  { icon: "👁️", theme: "fate-hierophant", svg: `<svg viewBox="0 0 100 150"><path d="M50 10 L10 140 L90 140 Z" stroke="#2a5c4d" stroke-width="2" fill="none"/></svg>` },
  { icon: "🔗", theme: "fate-lovers", svg: `<svg viewBox="0 0 100 150"><circle cx="40" cy="75" r="20" stroke="#ff0033" stroke-width="2" fill="none"/><circle cx="60" cy="75" r="20" stroke="#2a5c4d" stroke-width="2" fill="none"/></svg>` },
  { icon: "⚙️", theme: "fate-chariot", svg: `<svg viewBox="0 0 100 150"><circle cx="50" cy="75" r="40" stroke="#ffd700" fill="none" stroke-width="2" stroke-dasharray="5 5"/></svg>` },
  { icon: "💢", theme: "fate-strength", svg: `<svg viewBox="0 0 100 150"><path d="M10 10 L90 140 M90 10 L10 140" stroke="#ff0033" stroke-width="3" fill="none"/></svg>` },
  { icon: "🕯️", theme: "fate-hermit", svg: `<svg viewBox="0 0 100 150"><circle cx="50" cy="75" r="15" fill="#ffd700" filter="blur(5px)"/></svg>` },
  { icon: "🌀", theme: "fate-wheel", svg: `<svg viewBox="0 0 100 150"><circle cx="50" cy="75" r="40" stroke="#998550" stroke-width="2" fill="none"/><path d="M50 35 V115 M20 75 H80" stroke="#998550" stroke-width="2"/></svg>` },
  { icon: "⚔️", theme: "fate-justice", svg: `<svg viewBox="0 0 100 150"><path d="M50 10 V140 M10 40 H90" stroke="#998550" stroke-width="2" fill="none"/></svg>` },
  { icon: "⚓", theme: "fate-hanged", svg: `<svg viewBox="0 0 100 150"><path d="M50 10 V100 L20 140" stroke="#2a5c4d" stroke-width="2" fill="none"/></svg>` },
  { icon: "💀", theme: "fate-death", svg: `<svg viewBox="0 0 100 150"><path d="M20 140 Q50 10 80 140" stroke="#507a66" stroke-width="3" fill="none"/></svg>` },
  { icon: "🧪", theme: "fate-temperance", svg: `<svg viewBox="0 0 100 150"><path d="M30 20 Q50 80 70 20" stroke="#2a5c4d" stroke-width="2" fill="none"/></svg>` },
  { icon: "🔱", theme: "fate-devil", svg: `<svg viewBox="0 0 100 150"><path d="M50 140 L10 20 L90 20 Z" stroke="#ff0033" stroke-width="2" fill="none"/></svg>` },
  { icon: "⚡", theme: "fate-tower", svg: `<svg viewBox="0 0 100 150"><path d="M20 10 L80 140" stroke="#ff0033" stroke-width="4" fill="none"/></svg>` },
  { icon: "☘️", theme: "fate-star", svg: `<svg viewBox="0 0 100 150"><path d="M50 10 L60 60 L110 70 L60 80 L50 130 L40 80 L-10 70 L40 60 Z" fill="#ffd700"/></svg>` },
  { icon: "🌑", theme: "fate-moon", svg: `<svg viewBox="0 0 100 150"><circle cx="50" cy="75" r="35" fill="#2a5c4d" opacity="0.5"/></svg>` },
  { icon: "☀️", theme: "fate-sun", svg: `<svg viewBox="0 0 100 150"><circle cx="50" cy="75" r="25" fill="#000" stroke="#ffd700" stroke-width="2"/></svg>` },
  { icon: "🔔", theme: "fate-judgement", svg: `<svg viewBox="0 0 100 150"><path d="M10 20 L90 20 L50 130 Z" stroke="#998550" stroke-width="2" fill="none"/></svg>` },
  { icon: "💠", theme: "fate-world", svg: `<svg viewBox="0 0 100 150"><circle cx="50" cy="75" r="45" stroke="#2a5c4d" stroke-width="6" fill="none"/></svg>` }
];

const tarotDeck = tarotLore.cards.map((lore, index) => {
  return { ...lore, ...visualExtras[index] };
});

// ==========================================
// 🔮 全新逻辑：核心与三牌阵分离系统
// ==========================================
const activeFateTheme = ref("");
const crystalHighlight = ref(false), circleHighlight = ref(false), isGlitching = ref(false);

const isCoreRitualOpen = ref(false);
const isThreeCardRitualOpen = ref(false);
const isDestinyReadingOpen = ref(false);

const emptyCard = { name: "未确立", desc: "迷雾...", svg: "", icon: "", theme: "", isReversed: false };
const coreCardData = ref({...emptyCard});
const leftCardData = ref({...emptyCard});
const rightCardData = ref({...emptyCard});

const hasCoreRevealed = ref(false);
const hasLeftRevealed = ref(false);
const hasRightRevealed = ref(false);
const allRevealed = computed(() => hasCoreRevealed.value && hasLeftRevealed.value && hasRightRevealed.value);

const drawUniqueCard = (excludedNames) => {
  const available = tarotDeck.filter(c => !excludedNames.includes(c.name));
  const card = available[Math.floor(Math.random() * available.length)];
  const isRev = Math.random() > 0.5;
  return { 
    ...card, 
    isReversed: isRev,
    desc: isRev ? card.reversed.theme.split('、')[0] + '...' : card.upright.theme.split('、')[0] + '...'
  };
};

const openCoreRitual = () => { isCoreRitualOpen.value = true; crystalHighlight.value = false; };
const revealCoreCard = () => {
  if (hasCoreRevealed.value) return;
  const drawnNames = [];
  if (hasLeftRevealed.value) drawnNames.push(leftCardData.value.name);
  if (hasRightRevealed.value) drawnNames.push(rightCardData.value.name);
  coreCardData.value = drawUniqueCard(drawnNames);
  hasCoreRevealed.value = true;
  activeFateTheme.value = coreCardData.value.theme;
};
const closeCoreRitual = () => { isCoreRitualOpen.value = false; };

const warningMsg = ref("");
let warnTimer = null;
const showWarning = (msg) => {
  warningMsg.value = msg;
  if(warnTimer) clearTimeout(warnTimer);
  warnTimer = setTimeout(() => { warningMsg.value = ""; }, 2500);
};

const openThreeCardRitual = () => { isThreeCardRitualOpen.value = true; circleHighlight.value = false; };
const warnCoreUnflipped = () => {
  if (!hasCoreRevealed.value) showWarning("虚空未曾锚定... 请关闭法阵，先前往中央晶石抽取核心！");
};

const revealLeftCard = () => {
  if (!hasCoreRevealed.value) { warnCoreUnflipped(); return; }
  if (hasLeftRevealed.value) return;
  const drawnNames = [coreCardData.value.name];
  if (hasRightRevealed.value) drawnNames.push(rightCardData.value.name);
  leftCardData.value = drawUniqueCard(drawnNames);
  hasLeftRevealed.value = true;
};

const revealRightCard = () => {
  if (!hasCoreRevealed.value) { warnCoreUnflipped(); return; }
  if (hasRightRevealed.value) return;
  const drawnNames = [coreCardData.value.name];
  if (hasLeftRevealed.value) drawnNames.push(leftCardData.value.name);
  rightCardData.value = drawUniqueCard(drawnNames);
  hasRightRevealed.value = true;
};

const closeThreeCardRitual = () => { isThreeCardRitualOpen.value = false; };

const openDestinyReading = () => {
  isThreeCardRitualOpen.value = false;
  setTimeout(() => { isDestinyReadingOpen.value = true; }, 500);
};

const generateReadingText = (position, cardData) => {
  if (!cardData.name || cardData.name === "未确立") return "";
  const stateData = cardData.isReversed ? cardData.reversed : cardData.upright;
  
  if (position === 'core') {
    return `<p><strong>【核心主题】</strong> <span>${stateData.theme}</span></p>
            <p><strong>【解读基调】</strong> <span>${stateData.tone}</span></p>
            <p><strong>【人生课题】</strong> <span>${stateData.lesson}</span></p>`;
  } else if (position === 'left') {
    return `<p><strong>【因果渊源】</strong> <span>${stateData.left_source}</span></p>`;
  } else if (position === 'right') {
    return `<p><strong>【破局指引】</strong> <span>${stateData.right_guide}</span></p>`;
  }
};

// ==========================================
// 🌟 2秒思维畸变打字机
// ==========================================
const messages = ref([]), newMessage = ref(""), msgListRef = ref(null);
const glitchChars = "᚛ ᚑᚃᚅᚆᚇᚈᚉᚊᚋᚌᚍᚎᚏᚐᚑᚒᚓᚔᚕᚖᚗᚘᚙX￥%&*#@";

const submitMessage = async () => {
  const t = newMessage.value.trim(); if (!t) return;
  
  if (!hasCoreRevealed.value) {
    isBoardOpen.value = false; 
    isGlitching.value = true; 
    crystalHighlight.value = true;
    await nextTick();
    startCanvasSwarm(); 
    setTimeout(() => { isGlitching.value = false; stopCanvasSwarm(); }, 3000); 
    return;
  }

  const msgObj = reactive({ id: Date.now(), author: coreCardData.value.name, icon: coreCardData.value.icon, text: t, displayText: "" });
  messages.value.push(msgObj); 
  newMessage.value = "";
  
  let elapsed = 0;
  const totalDuration = 2000; 
  const interval = setInterval(() => {
    elapsed += 30;
    if (elapsed >= totalDuration) {
      clearInterval(interval);
      msgObj.displayText = t; 
    } else {
      const ratio = elapsed / totalDuration;
      msgObj.displayText = t.split("").map((char, index) => {
        if (index < Math.floor(ratio * t.length)) return t[index];
        return glitchChars[Math.floor(Math.random() * glitchChars.length)];
      }).join("");
    }
  }, 30);
  
  await nextTick(); if (msgListRef.value) msgListRef.value.scrollTop = msgListRef.value.scrollHeight;
};

// --- 面板控制 ---
const isBoardOpen = ref(false), isTearing = ref(false), isDevPanelOpen = ref(false);
const triggerSpaceTear = () => { isTearing.value = true; isBoardOpen.value = true; };
const closeSpaceTear = () => { isBoardOpen.value = false; setTimeout(() => isTearing.value = false, 1000); };
const cleanUpTearFilter = () => isTearing.value = false;
const toggleDevPanel = () => isDevPanelOpen.value = !isDevPanelOpen.value;
</script>

<style>
/* ================= 全局基础 ================= */
* { box-sizing: border-box; cursor: none !important; } 
html, body { margin: 0; padding: 0; width: 100vw; height: 100vh; overflow: hidden; background: #000; }
#app { margin: 0 !important; padding: 0 !important; max-width: none !important; width: 100vw; height: 100vh; text-align: left; }
#vzuor-absolute-frame { position: relative; width: 100%; height: 100%; background: #000; overflow: hidden; }

/* 🌟 0延迟原生光标样式 🌟 */
.custom-cursor-core { position: fixed; width: 6px; height: 6px; background: #ffcc00; border-radius: 50%; box-shadow: 0 0 15px #ff0033, 0 0 5px #ffcc00; pointer-events: none; z-index: 100000; top: -3px; left: -3px; }
.native-cursor-trail { position: fixed; width: 8px; height: 8px; background: #ff0033; border-radius: 50%; pointer-events: none; z-index: 99999; top: -4px; left: -4px; mix-blend-mode: screen; filter: blur(1px); }

/* 🚨 惩罚地震特效 🚨 */
.screen-shake { animation: violentShake 0.1s infinite; }
@keyframes violentShake { 0% { transform: translate(4px, 4px) rotate(0deg); } 25% { transform: translate(-4px, -4px) rotate(1deg); } 50% { transform: translate(4px, -4px) rotate(-1deg); } 75% { transform: translate(-4px, 4px) rotate(0deg); } 100% { transform: translate(0, 0) rotate(0deg); } }

/* 🌟 空间色散重影 🌟 */
.is-tearing-chromatic::before, .is-tearing-chromatic::after { content: ''; position: absolute; inset: 0; background: inherit; pointer-events: none; z-index: 5; mix-blend-mode: screen; opacity: 0.4; transition: 1s; }
.is-tearing-chromatic::before { transform: scale(1.01) translate(-4px, 0); filter: hue-rotate(-90deg) saturate(2); }
.is-tearing-chromatic::after { transform: scale(1.01) translate(4px, 0); filter: hue-rotate(90deg) saturate(2); }

/* 🌟 22种专属精神污染滤镜 🌟 */
.liquid-distort-container { position: relative; width: 100%; height: 100%; filter: url(#full-liquid-breathing); transition: all 2s ease; }
.fate-fool { filter: url(#full-liquid-breathing) sepia(0.5) saturate(1.3) brightness(0.9); }
.fate-magician { filter: url(#full-liquid-breathing) hue-rotate(220deg) brightness(1.1) saturate(1.4); }
.fate-priestess { filter: url(#full-liquid-breathing) brightness(0.4) saturate(0.5) contrast(1.5); }
.fate-empress { filter: url(#full-liquid-breathing) sepia(0.8) saturate(3) brightness(0.7) hue-rotate(-30deg); }
.fate-emperor { filter: url(#full-liquid-breathing) grayscale(0.7) brightness(0.6) contrast(2); }
.fate-hierophant { filter: url(#full-liquid-breathing) hue-rotate(60deg) brightness(1.3) contrast(1.2) sepia(0.2); }
.fate-lovers { filter: url(#full-liquid-breathing) hue-rotate(-20deg) contrast(1.4) saturate(1.2) blur(1px); }
.fate-chariot { animation: fateChariotPanic 1s infinite; } 
.fate-strength { filter: url(#full-liquid-breathing) sepia(1) saturate(4) brightness(0.6) hue-rotate(-50deg); }
.fate-hermit { filter: url(#full-liquid-breathing) brightness(0.3) saturate(0.2) contrast(1.3); }
.fate-wheel { filter: url(#full-liquid-breathing) contrast(1.8) saturate(1.1) invert(0.05); }
.fate-justice { filter: url(#full-liquid-breathing) grayscale(1) brightness(0.8) contrast(1.1) invert(0.1); }
.fate-hanged { filter: url(#full-liquid-breathing) hue-rotate(-60deg) brightness(0.7) contrast(1.1); }
.fate-death { filter: url(#full-liquid-breathing) grayscale(0.9) brightness(0.5) contrast(1.2); }
.fate-temperance { filter: url(#full-liquid-breathing) hue-rotate(100deg) contrast(1.2) blur(2px); }
.fate-devil { filter: url(#full-liquid-breathing) sepia(1) saturate(5) brightness(0.4) hue-rotate(-70deg); }
.fate-tower { filter: url(#full-liquid-breathing) contrast(3) brightness(1.5) grayscale(0.5); }
.fate-star { filter: url(#full-liquid-breathing) hue-rotate(180deg) brightness(1.2) blur(3px) saturate(2); }
.fate-moon { filter: url(#full-liquid-breathing) hue-rotate(130deg) contrast(1.3) saturate(0.8); }
.fate-sun { filter: url(#full-liquid-breathing) invert(0.9) hue-rotate(180deg) contrast(1.5); }
.fate-judgement { filter: url(#full-liquid-breathing) sepia(0.3) saturate(0.6) contrast(1.4) hue-rotate(30deg); }
.fate-world { filter: url(#full-liquid-breathing) grayscale(0.5) blur(10px) contrast(0.8) brightness(0.6); } 
@keyframes fateChariotPanic { 0%, 100% { filter: url(#full-liquid-breathing) hue-rotate(0deg) invert(0); } 50% { filter: url(#full-liquid-breathing) hue-rotate(180deg) invert(1); } }

/* ================= 布局控制 ================= */
.wing { position: absolute; top: 0; height: 100%; width: 30vw; z-index: 1; pointer-events: none; }
.liquid-image { width: 100%; height: 100%; object-fit: cover; display: block; }
.left-wing { left: 0; } .right-wing { right: 0; }
.center-main { position: absolute; top: 0; left: 25vw; width: 50vw; height: 100%; z-index: 2; -webkit-mask-image: linear-gradient(to right, transparent 0%, black 10%, black 90%, transparent 100%); mask-image: linear-gradient(to right, transparent 0%, black 10%, black 90%, transparent 100%); }
.inverted { transform: rotate(180deg); }
.left-ui-layer, .right-ui-layer, .center-ui-layer { position: absolute; top: 0; width: 30vw; height: 100%; z-index: 10; pointer-events: none; }
.left-ui-layer { left: 0; } .right-ui-layer { right: 0; } .center-ui-layer { left: 25vw; width: 50vw; }

/* 🌟 神之眼 🌟 */
.eye-deep-core { position: absolute; transform: translate(-50%, -50%); aspect-ratio: 1 / 1; border-radius: 50%; background-color: #05000a; box-shadow: inset 0 0 25px rgba(0,0,0,1), 0 0 40px rgba(0,0,0,0.9); overflow: hidden; display: flex; justify-content: center; align-items: center; pointer-events: auto; }
.void-ring { position: absolute; width: 85%; height: 85%; border-radius: 50%; border: 1px solid rgba(80, 0, 150, 0.3); box-shadow: inset 0 0 15px rgba(80, 0, 150, 0.2), 0 0 15px rgba(255, 0, 50, 0.1); }
.pupil-nebula { position: absolute; top: 50%; left: 50%; width: 40%; height: 40%; border-radius: 50%; background: radial-gradient(circle, #ff0000 0%, #aa0000 30%, #220044 80%, #000 100%); box-shadow: 0 0 20px rgba(255, 0, 0, 0.5); display: flex; justify-content: center; align-items: center; }
.pupil-core-vertical { width: 6px; background: #fff; border-radius: 50%; box-shadow: 0 0 10px #fff, 0 0 20px #ff0000; transition: height 0.05s ease-out; }

/* ================= 交互按钮 ================= */
.flesh-scar-btn, .void-crystal-btn, .destiny-circle-btn { position: absolute; transform: translate(-50%, -50%); pointer-events: auto; mix-blend-mode: color-dodge; overflow: visible; display: flex; flex-direction: column; align-items: center; justify-content: center; }
.sigil-text { position: absolute; top: 110%; font-family: monospace; font-size: 0.8vw; color: #998550; opacity: 0; letter-spacing: 0.3vw; transition: all 0.3s; pointer-events: none; }
.flesh-scar-btn:hover .sigil-text, .void-crystal-btn:hover .sigil-text, .destiny-circle-btn:hover .sigil-text { opacity: 1; top: 90%; }

.flesh-scar-btn { width: 5vw; height: 5vw; }
.abyssal-sigil { width: 100%; height: 100%; transform: rotate(-15deg); transition: all 0.3s ease; filter: drop-shadow(0 0 5px rgba(42, 92, 77, 0.4)); }
.flesh-scar-btn:hover .abyssal-sigil { transform: rotate(-15deg) scale(1.15); filter: drop-shadow(0 0 15px #428a75); }

.void-crystal-btn { width: 6vw; height: 9vw; z-index: 20; }
.crystal-svg { width: 100%; height: 100%; animation: crystalFloat 4s infinite ease-in-out; transition: all 0.3s; filter: drop-shadow(0 0 5px rgba(42, 92, 77, 0.5)); }
.crystal-eye { animation: coreGlow 2.5s infinite alternate ease-in-out; }
.void-crystal-btn:hover .crystal-svg { filter: drop-shadow(0 0 20px #998550); transform: scale(1.1); }
@keyframes crystalFloat { 0%, 100% { transform: translateY(0) rotate(0deg); } 50% { transform: translateY(-10px) rotate(3deg); } }
@keyframes coreGlow { 0% { r: 3; fill: #507a66; } 100% { r: 6; fill: #998550; } }

/* 🌟 右翼法阵 UI 🌟 */
.destiny-circle-btn { width: 7vw; height: 7vw; }
.magic-circle-svg { width: 100%; height: 100%; filter: drop-shadow(0 0 5px rgba(153, 133, 80, 0.5)); transition: all 0.3s ease; }
.spin-slow { transform-origin: center; animation: spin 15s linear infinite; }
.spin-fast-reverse { transform-origin: center; animation: spin 8s linear infinite reverse; }
.pulse-alpha { animation: pulseAlpha 3s infinite alternate ease-in-out; }
.circle-core-eye { animation: coreGlow 2s infinite alternate ease-in-out; }
.destiny-circle-btn:hover .magic-circle-svg { filter: drop-shadow(0 0 20px #ff0033); transform: scale(1.1); }
@keyframes spin { 100% { transform: rotate(360deg); } }
@keyframes pulseAlpha { 0% { opacity: 0.3; stroke-width: 1; } 100% { opacity: 1; stroke-width: 2.5; } }

/* 🚨 UI抽搐暴走状态 🚨 */
.is-freaking-out svg { animation: freakOut 0.1s infinite !important; filter: drop-shadow(0 0 30px #ff0033) hue-rotate(-30deg) !important; }
.is-freaking-out .crystal-eye, .is-freaking-out .circle-core-eye { fill: #ff0033 !important; r: 12 !important; }
.glitch-label { opacity: 1 !important; top: 85% !important; color: #ff0033 !important; font-weight: bold; font-size: 1.2vw !important; text-shadow: 0 0 15px #ff0033; animation: textTwitch 0.1s infinite; }
@keyframes freakOut { 0% { transform: translate(0,0) scale(1.3) skew(0deg); } 25% { transform: translate(-8px,5px) scale(1.35) skew(15deg); } 50% { transform: translate(8px,-5px) scale(1.2) skew(-15deg); } 75% { transform: translate(-5px,-8px) scale(1.4) skew(10deg); } 100% { transform: translate(5px,8px) scale(1.3) skew(-10deg); } }
@keyframes textTwitch { 0% { transform: translateX(-50%) translate(3px, 0); } 100% { transform: translateX(-50%) translate(-3px, 0); } }

/* ================= 高性能 Canvas THE THOUSAND-EYED VOID ================= */
.eldritch-terror-overlay { position: fixed; inset: 0; z-index: 99999; background: #010000; display: flex; flex-direction: column; align-items: center; justify-content: center; overflow: hidden; animation: crtFlicker 0.1s infinite; cursor: none !important; }
.terror-vignette { position: absolute; inset: 0; background: radial-gradient(circle, transparent 10%, rgba(120,0,0,0.7) 70%, #000 100%); pointer-events: none; z-index: 2; mix-blend-mode: multiply; }

.swarm-canvas-layer { position: absolute; inset: 0; width: 100vw; height: 100vh; z-index: 0; }
.terror-eye { position: absolute; width: 90vw; height: 90vh; opacity: 0; z-index: 1; display: flex; justify-content: center; align-items: center; animation: spawnEye 0.1s forwards; filter: drop-shadow(0 0 100px rgba(255,0,0,0.8)); }
.terror-eye svg { width: 100%; height: 100%; }
.blood-veins { animation: crawl 0.3s infinite linear; }

@keyframes spawnEye { 0% { opacity: 0; transform: scale(0.1) rotate(0deg); } 100% { opacity: 0.9; transform: scale(1); } }
@keyframes crawl { 100% { stroke-dashoffset: 40; } }
@keyframes crtFlicker { 0% { background: #050000; } 50% { background: #330000; } 100% { background: #000; } }

/* ================= 启示录 Modal ================= */
.message-board-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: radial-gradient(circle at 50% 50%, rgba(2, 8, 6, 0.8) 0%, rgba(0, 0, 0, 0.98) 100%); backdrop-filter: blur(10px); z-index: 1000; display: flex; justify-content: center; align-items: center; }
.message-board-overlay.is-tearing { filter: url(#space-tear-filter); }
.cthulhu-modal { width: 55vw; max-width: 800px; padding: 40px; color: #7a998f; font-family: serif; background: rgba(0,0,0,0.4); box-shadow: inset 0 0 100px #000; position: relative; }

.close-btn { position: absolute; top: 20px; right: 30px; background: none; border: none; color: rgba(80, 122, 102, 0.5); font-size: 32px; transition: all 0.3s; z-index: 10; cursor: none !important; }
.close-btn:hover { transform: scale(1.2) rotate(180deg); color: #998550; text-shadow: 0 0 15px #998550; }
.modal-header { text-align: center; margin-bottom: 30px; }
.ancient-title { color: #998550; font-size: 32px; letter-spacing: 1.5vw; font-weight: normal; text-shadow: 0 0 15px rgba(153, 133, 80, 0.5), 0 0 30px rgba(0,0,0,1); margin: 0 0 15px 0; opacity: 0.9; }
.eldritch-divider { height: 1px; width: 60%; margin: 0 auto; background: radial-gradient(ellipse at center, rgba(153, 133, 80, 0.4) 0%, transparent 80%); }

.messages-list-wrapper { position: relative; -webkit-mask-image: linear-gradient(to bottom, transparent 0%, black 15%, black 85%, transparent 100%); mask-image: linear-gradient(to bottom, transparent 0%, black 15%, black 85%, transparent 100%); }
.messages-list { height: 300px; overflow-y: auto; overflow-x: hidden; padding: 40px 20px; scrollbar-width: none; }
.messages-list::-webkit-scrollbar { display: none; }
.empty-state { text-align: center; color: #3a5248; font-style: italic; letter-spacing: 0.5vw; margin-top: 80px; opacity: 0.6; }

.msg-item { display: flex; align-items: flex-start; gap: 10px; margin-bottom: 20px; font-size: 16px; border-left: 2px solid #2a5c4d; padding-left: 15px; }
.fate-icon { font-size: 14px; filter: drop-shadow(0 0 5px rgba(255,215,0,0.5)); opacity: 0.9; }
.msg-author { color: #c2a366; font-weight: bold; min-width: max-content; }
.msg-text { color: #8ba69d; line-height: 1.6; text-shadow: 0 0 5px rgba(0,0,0,0.5); word-break: break-all; }

.msg-fade-enter-active, .msg-fade-leave-active { transition: all 1s ease; }
.msg-fade-enter-from { opacity: 0; filter: blur(8px); transform: translateY(20px); }

.input-area { display: flex; flex-direction: column; gap: 10px; position: relative; z-index: 2; margin-top: 10px; }
.abyssal-textarea { width: 100%; height: 60px; background: rgba(0, 0, 0, 0.4); box-shadow: inset 0 5px 15px rgba(0,0,0,0.8); border: none; border-bottom: 1px solid rgba(42, 92, 77, 0.3); color: #998550; padding: 15px 20px; resize: none; outline: none; font-family: inherit; font-size: 15px; letter-spacing: 1px; transition: all 0.5s ease; cursor: none !important; }
.abyssal-textarea::placeholder { color: #2a3d34; font-style: italic; letter-spacing: 2px;}
.abyssal-textarea:focus { background: rgba(2, 15, 10, 0.6); border-bottom: 1px solid rgba(153, 133, 80, 0.6); height: 100px; }
.submit-btn { align-self: center; background: transparent; color: #507a66; border: none; padding: 10px 40px; font-family: inherit; font-size: 14px; letter-spacing: 1vw; transition: all 0.5s ease; opacity: 0.7; cursor: none !important; }
.submit-btn:hover { color: #c2a366; opacity: 1; text-shadow: 0 0 20px rgba(194, 163, 102, 0.8); transform: scale(1.05); }
.anonymous-notice { text-align: right; color: #9e1b1b; font-size: 13px; margin-top: 15px; font-style: italic; letter-spacing: 2px; text-shadow: 1px 1px 2px #000, 0 0 8px rgba(158, 27, 27, 0.6); opacity: 0.9; }

.space-rip-enter-active { animation: spaceRipOpen 1.2s cubic-bezier(0.8, 0, 0.2, 1) forwards; }
.space-rip-leave-active { animation: spaceRipOpen 0.8s cubic-bezier(0.8, 0, 0.2, 1) reverse forwards; }
@keyframes spaceRipOpen { 0% { clip-path: polygon(50% 50%, 50% 50%, 50% 50%, 50% 50%); opacity: 0; filter: brightness(0) contrast(200%); } 100% { clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%); opacity: 1; filter: brightness(1) contrast(100%); } }

/* ================= 🔮 塔罗仪式面板 (单牌与三牌阵通用) ================= */
.tarot-ritual-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.95); z-index: 2000; display: flex; align-items: center; justify-content: center; backdrop-filter: blur(10px); cursor: none !important; }
.void-dissolve-enter-active, .void-dissolve-leave-active { transition: opacity 0.5s ease; }
.void-dissolve-enter-from, .void-dissolve-leave-to { opacity: 0; }
.ritual-stage { text-align: center; position: relative; }
.ritual-title { color: #998550; font-size: 24px; letter-spacing: 8px; margin-bottom: 30px; text-shadow: 0 0 15px rgba(153,133,80,0.5); }

/* 🌟 三牌阵专属横向布局 🌟 */
.three-cards-stage { max-width: 90vw; }
.cards-layout { display: flex; justify-content: center; align-items: center; gap: 40px; margin-bottom: 30px; }
.card-slot { display: flex; flex-direction: column; align-items: center; }
.slot-title { color: #c2a366; font-size: 14px; letter-spacing: 4px; margin-bottom: 15px; text-shadow: 0 0 10px rgba(194, 163, 102, 0.5); }
.core-slot { transform: scale(1.15); margin: 0 10px; z-index: 2; } /* 中位核心牌微放大 */
.core-title { color: #ff0033; text-shadow: 0 0 10px #ff0033; font-weight: bold; }

.tarot-card-3d { width: 160px; height: 260px; perspective: 1000px; margin: 0 auto; cursor: none !important; transform-style: preserve-3d; transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1); }
.core-slot .tarot-card-3d { width: 180px; height: 290px; } /* 核心牌物理变大 */

.tarot-card-3d.flipped { transform: rotateY(180deg); }
.card-face { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; border: 2px solid #998550; border-radius: 10px; background: #050505; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 15px; box-shadow: 0 0 30px rgba(0,0,0,0.8); }
.card-back { background: repeating-linear-gradient(45deg, #050505, #050505 5px, #0a1f18 5px, #0a1f18 6px); }
.card-front { transform: rotateY(180deg); }

/* 🌟 正逆位图形畸变 🌟 */
.card-svg-wrap { width: 80px; height: 120px; margin-bottom: 15px; filter: drop-shadow(0 0 5px #ff0033); transition: transform 0.5s ease; }
.reversed-svg { transform: rotate(180deg); filter: drop-shadow(0 0 8px #ff0033) hue-rotate(45deg); }
.core-slot .card-svg-wrap { width: 100px; height: 150px; }

.card-name { font-size: 18px; color: #ffd700; margin-bottom: 10px; font-weight: bold; text-align: center; }
.card-state { font-size: 14px; color: #ff0033; margin-left: 5px; opacity: 0.8; font-weight: normal; }
.card-desc { font-size: 11px; color: #8ba69d; line-height: 1.5; text-align: center; padding: 0 5px; }

.ritual-hint { color: #998550; opacity: 0.6; font-size: 12px; margin-top: 20px; letter-spacing: 2px; }
.blink-hint { animation: pulseText 2s infinite; }
@keyframes pulseText { 0%, 100% { opacity: 0.3; } 50% { opacity: 0.8; } }

.validation-msg { color: #ff0033; font-size: 13px; font-style: italic; text-shadow: 0 0 10px #ff0033; letter-spacing: 2px; margin-top: 20px; animation: pulseRed 1s infinite alternate; }
@keyframes pulseRed { 0% { opacity: 0.7; } 100% { opacity: 1; text-shadow: 0 0 20px #ff0033; } }

.accept-btn { background: transparent; border: 1px solid #ff0033; color: #ff0033; padding: 12px 45px; letter-spacing: 6px; transition: 0.3s; cursor: none !important; margin-top: 20px; font-weight: bold; text-shadow: 0 0 10px #ff0033; }
.accept-btn:hover { background: rgba(255,0,51,0.2); color: #fff; box-shadow: inset 0 0 15px #ff0033, 0 0 25px #ff0033; }

/* ================= 📜 极度扭曲的活体血肉面板 (Abyssal Flesh Reading) ================= */
/* 确保覆盖层满屏且正确居中 */
.destiny-reading-overlay { 
  position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
  display: flex; align-items: center; justify-content: center;
  z-index: 2500; backdrop-filter: blur(20px) brightness(0.2); 
}

/* 容器：非矩形的蠕动肉块 */
.flesh-modal { 
  position: relative;
  max-width: 900px; width: 85vw; max-height: 85vh; 
  background: #070002;
  /* 利用不规则的圆角产生扭曲感 */
  border-radius: 2% 4% 3% 5% / 5% 3% 4% 2%;
  box-shadow: inset 0 0 60px #2a0005, 0 0 40px rgba(139, 0, 0, 0.4);
  display: flex; flex-direction: column; overflow: hidden;
  animation: fleshBreathing 8s infinite alternate ease-in-out;
  border: 1px solid #4a0008;
}

@keyframes fleshBreathing {
  0% { border-radius: 2% 4% 3% 5% / 5% 3% 4% 2%; transform: scale(1) rotate(0.2deg); box-shadow: inset 0 0 60px #2a0005, 0 0 40px rgba(139, 0, 0, 0.4); }
  100% { border-radius: 5% 2% 4% 3% / 2% 5% 3% 4%; transform: scale(1.02) rotate(-0.2deg); box-shadow: inset 0 0 100px #4a0008, 0 0 80px rgba(200, 0, 0, 0.2); }
}

/* 底层噪点与微弱血丝 */
.flesh-modal-bg-texture {
  position: absolute; inset: 0; pointer-events: none;
  background-image: radial-gradient(circle at center, transparent 40%, #000 100%), repeating-linear-gradient(45deg, rgba(255,0,0,0.03) 0px, rgba(255,0,0,0.03) 2px, transparent 2px, transparent 4px);
  filter: url(#space-tear-filter);
  opacity: 0.4;
  z-index: 0;
}

/* 关闭按钮异化 */
.mad-close { position: absolute; top: 20px; right: 30px; background: none; border: none; color: rgba(255,0,0,0.5); z-index: 10; font-size: 40px; cursor: none !important; transition: all 0.3s; }
.mad-close:hover { color: #ff0033; text-shadow: 0 0 20px #ff0033; filter: url(#text-madness); transform: scale(1.2) rotate(180deg); }

/* 标题严重色散 */
.madness-header { margin-bottom: 20px; padding-top: 20px; text-align: center; z-index: 2; position: relative; }
.twisted-text { 
  position: relative; color: #8b0000; font-size: 38px; font-weight: bold; letter-spacing: 2vw;
  text-shadow: 2px 0px 4px rgba(255,0,0,0.8), -2px 0px 4px rgba(0,255,255,0.2); 
  margin: 0;
}
.twisted-text::before, .twisted-text::after { 
  content: attr(data-text); position: absolute; top: 0; left: 0; width: 100%; height: 100%; opacity: 0.6; mix-blend-mode: screen;
}
.twisted-text::before { left: 3px; text-shadow: -3px 0 red; animation: glitch-mad-1 2s infinite linear alternate-reverse; }
.twisted-text::after { left: -3px; text-shadow: -3px 0 cyan; animation: glitch-mad-2 3s infinite linear alternate-reverse; }
@keyframes glitch-mad-1 { 0% { clip-path: inset(20% 0 80% 0); transform: skewX(2deg); } 100% { clip-path: inset(50% 0 30% 0); transform: skewX(-2deg); } }
@keyframes glitch-mad-2 { 0% { clip-path: inset(10% 0 60% 0); transform: skewY(1deg); } 100% { clip-path: inset(80% 0 5% 0); transform: skewY(-1deg); } }

.madness-content { overflow-y: auto; padding: 10px 25px 30px 25px; scrollbar-width: none; z-index: 2; position: relative; }
.madness-content::-webkit-scrollbar { display: none; }

/* 撕裂状的边框 */
.flesh-section { 
  position: relative; margin-bottom: 40px; padding: 20px 30px; 
  border-left: 3px solid transparent; 
  border-image: linear-gradient(to bottom, transparent, #8b0000, transparent) 1;
  background: linear-gradient(90deg, rgba(80,0,10,0.6) 0%, transparent 100%); 
  mask-image: linear-gradient(to right, black 80%, transparent 100%);
  -webkit-mask-image: linear-gradient(to right, black 80%, transparent 100%);
}

.mad-section-title { color: #ff3333; font-size: 18px; letter-spacing: 2px; margin-top: 0; margin-bottom: 15px; display: flex; justify-content: space-between; border-bottom: 1px solid rgba(139,0,0,0.5); padding-bottom: 8px; filter: drop-shadow(0 0 5px #ff0000); }
.chromatic-bleed { color: #8b0000; font-size: 14px; text-shadow: 1px 0px 2px rgba(255,0,0,1), -1px 0px 2px rgba(0,255,255,0.4); font-weight: bold; }

/* 文字渗血浮现动画 (极度扭曲) */
.mad-text { color: #8ba69d; line-height: 1.9; font-size: 15px; }
.mad-text p { 
  margin: 10px 0; 
  opacity: 0; 
  animation: madBleedIn 2.5s cubic-bezier(0.1, 0.9, 0.2, 1) forwards; 
}
.mad-text p:nth-child(1) { animation-delay: 0.2s; }
.mad-text p:nth-child(2) { animation-delay: 0.6s; }
.mad-text p:nth-child(3) { animation-delay: 1.0s; }

@keyframes madBleedIn { 
  0% { opacity: 0; filter: blur(20px) contrast(300%) drop-shadow(0 0 20px red); transform: skewX(20deg) scaleY(2) translateY(20px); color: #8b0000; } 
  50% { opacity: 0.8; filter: blur(3px) contrast(150%) drop-shadow(0 0 5px darkred); transform: skewX(-5deg) scaleY(1.1) translateY(0); color: #ff3333; } 
  100% { opacity: 1; filter: blur(0) contrast(100%); transform: skewX(0) scaleY(1); color: #8ba69d; } 
}

.mad-text strong { 
  color: #998550; font-family: serif; font-size: 1.1em; 
  text-shadow: 1px 0px 2px rgba(255,0,0,0.8), -1px 0px 2px rgba(0,255,0,0.2); 
}

.final-void { border-left-color: #ff0033; background: linear-gradient(90deg, rgba(255,0,0,0.05) 0%, transparent 100%); font-style: italic; }
.final-void .mad-section-title { border-bottom: none; }
.text-center { text-align: center; }

/* ================= 控制台 ================= */
.blind-spot-trigger { position: absolute; top: 0; left: 0; width: 40px; height: 40px; z-index: 9999; opacity: 0; transition: opacity 0.3s; cursor: none !important; }
.blind-spot-trigger:hover { background: radial-gradient(circle, rgba(255,0,0,0.3) 0%, transparent 60%); opacity: 1; }
.dev-terminal { position: absolute; top: 50px; left: 20px; z-index: 10000; background: rgba(10, 0, 0, 0.95); border: 1px solid #ff0033; padding: 15px; width: 300px; color: #ff3333; font-family: monospace; font-size: 12px; box-shadow: 0 0 20px rgba(255,0,0,0.3); backdrop-filter: blur(5px); }
.dev-terminal * { cursor: default !important; } 
.terminal-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px dashed #ff0033; padding-bottom: 8px; margin-bottom: 12px; }
.sys-close { background: none; border: none; color: #ff3333; font-weight: bold; }
.sys-close:hover { background: #ff3333; color: #000; }
.sys-group { margin-bottom: 12px; display: flex; align-items: center; justify-content: space-between; }
.sys-group label { margin-bottom: 0; }
.sys-group input[type="range"] { width: 80px; accent-color: #ff0033; }
.sys-divider { border: 0; border-bottom: 1px dashed #ff0033; margin: 15px 0; opacity: 0.5; }
.reset-sys-btn { width: 100%; margin-top: 10px; background: #330000; border: 1px solid #ff0033; color: #ff9999; padding: 5px; }
.reset-sys-btn:hover { background: #660000; color: #fff; }
.fade-sys-enter-active, .fade-sys-leave-active { transition: opacity 0.2s; }
.fade-sys-enter-from, .fade-sys-leave-to { opacity: 0; }
</style>