<template>
  <div id="vzuor-absolute-frame">
    <img :src="imgZuo" class="wing left-wing inverted" />
    
    <img :src="imgYou" class="wing right-wing inverted" />

    <img :src="imgZhong" class="center-main" />
  </div>
</template>

<script setup>
import imgZhong from './assets/zhong.jpg'
import imgZuo from './assets/zuo1.jpg'
import imgYou from './assets/you2.jpg'
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #000;
}

#vzuor-absolute-frame {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: #000;
}

/* 侧边两翼占 30% 宽度（比原来的 25% 宽，能保留更多内容），置于底层 */
.wing {
  position: absolute;
  top: 0;
  height: 100%;
  width: 30vw;
  object-fit: cover;
  z-index: 1;
  pointer-events: none;
}
.left-wing { left: 0; }
.right-wing { right: 0; }

/* 中间主图占 50% 宽度，放在上层 */
.center-main {
  position: absolute;
  top: 0;
  left: 25vw; /* 刚好压住左侧 5vw，压住右侧 5vw */
  width: 50vw;
  height: 100%;
  object-fit: cover;
  z-index: 2;
  pointer-events: none;
  
  /* 核心融合魔法：只让中间图的左右边缘向内羽化透明，露出底下的图 */
  -webkit-mask-image: linear-gradient(to right, 
    transparent 0%, 
    black 10%, 
    black 90%, 
    transparent 100%);
  mask-image: linear-gradient(to right, 
    transparent 0%, 
    black 10%, 
    black 90%, 
    transparent 100%);
}

/* 物理倒置 */
.inverted {
  transform: rotate(180deg);
}
</style>