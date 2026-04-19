<template>
  <div id="vzuor-absolute-frame">
    <div class="ambiance-overlay"></div>

    <div class="col left-wing">
      <img :src="imgZuo" class="absolute-fill inverted mask-to-right" />
    </div>

    <div class="col center-main">
      <img :src="imgZhong" class="absolute-fill mask-both-sides" />
    </div>

    <div class="col right-wing">
      <img :src="imgYou" class="absolute-fill inverted mask-to-left" />
    </div>
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
  background: #000; /* 背景深黑色，辅助羽化 */
}

#app {
  width: 100%;
  height: 100%;
}

#vzuor-absolute-frame {
  display: flex;
  width: 100vw;
  height: 100vh;
  align-items: stretch;
  background-color: #050505;
}

/* 核心比例分配 */
.left-wing { flex: 2.5; z-index: 1; }
.center-main { flex: 5; z-index: 2; margin: 0 -2%; } /* 增加 -2% 的负边距，让中间图稍微“入侵”两侧 */
.right-wing { flex: 2.5; z-index: 1; }

.col {
  height: 100%;
  position: relative;
  /* 取消 overflow: hidden; 允许图片边缘在重叠区稍微溢出 */
}

.absolute-fill {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  pointer-events: none;
}

/* --- 融合黑魔法：遮罩算法 --- */

/* 左翼：向右侧羽化消失 */
.mask-to-right {
  -webkit-mask-image: linear-gradient(to right, rgba(0,0,0,1) 85%, rgba(0,0,0,0) 100%);
  mask-image: linear-gradient(to right, rgba(0,0,0,1) 85%, rgba(0,0,0,0) 100%);
}

/* 右翼：向左侧羽化消失 */
.mask-to-left {
  -webkit-mask-image: linear-gradient(to left, rgba(0,0,0,1) 85%, rgba(0,0,0,0) 100%);
  mask-image: linear-gradient(to left, rgba(0,0,0,1) 85%, rgba(0,0,0,0) 100%);
}

/* 中间：双侧羽化，制造“嵌入”感 */
.mask-both-sides {
  -webkit-mask-image: linear-gradient(to right, 
    rgba(0,0,0,0) 0%, 
    rgba(0,0,0,1) 10%, 
    rgba(0,0,0,1) 90%, 
    rgba(0,0,0,0) 100%
  );
  mask-image: linear-gradient(to right, 
    rgba(0,0,0,0) 0%, 
    rgba(0,0,0,1) 10%, 
    rgba(0,0,0,1) 90%, 
    rgba(0,0,0,0) 100%
  );
}

/* 氛围层：在图片上方加一层暗角，压低拼接处的亮度，增加融合感 */
.ambiance-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
  pointer-events: none;
  background: radial-gradient(circle, rgba(0,0,0,0) 40%, rgba(0,0,0,0.4) 100%);
  box-shadow: inset 0 0 100px rgba(0,0,0,0.8);
}

/* 物理倒置保持不变 */
.inverted {
  transform: rotate(180deg);
}
</style>