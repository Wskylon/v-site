<template>
  <div id="vzuor-absolute-frame">
    <div class="ambiance-overlay-advanced"></div>

    <div class="col left-wing z-invasion">
      <img :src="imgZuo" class="absolute-fill inverted mask-organ-zuo blend-invasion" />
    </div>

    <div class="col center-main z-background">
      <img :src="imgZhong" class="absolute-fill mask-organ-zhong" />
    </div>

    <div class="col right-wing z-invasion">
      <img :src="imgYou" class="absolute-fill inverted mask-organ-you blend-invasion" />
    </div>
  </div>
</template>

<script setup>
import imgZhong from './assets/zhong.jpg'
import imgZuo from './assets/zuo1.jpg'
import imgYou from './assets/you2.jpg'
</script>

<style>
/* 关键：清除浏览器所有默认边距，防止产生白边或滚动条 */
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden; /* 物理切断滚动条 */
  background: #000;
}

#app {
  width: 100%;
  height: 100%;
}

#vzuor-absolute-frame {
  display: flex;
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  align-items: stretch;
  background-color: #050505; /* 背景极黑色，辅助羽化 */
}

/* 核心比例分配保持不变 */
.left-wing { flex: 2.5; position: relative; }
.center-main { flex: 5; position: relative; }
.right-wing { flex: 2.5; position: relative; }

/* 层级控制：让两侧“入侵”中间图，中間圖置於最底層 */
.z-invasion { z-index: 2; }
.z-background { z-index: 1; }

.col {
  height: 100%;
  /* 移除 overflow: hidden; 允许图片边缘在重叠区稍微溢出 */
}

/* 物理占满算法：确保内容不丢失，使用 contain */
.absolute-fill {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: contain; /* 关键：完整保留所有内容，不裁切 */
  display: block;
  pointer-events: none;
  transform-origin: center; /* 旋转中心 */
}

/* 物理倒置保持不变 */
.inverted {
  transform: rotate(180deg);
}


/* --- 三相深度融合协议 II：深度渗透与层级融合算法 --- */

/* 左翼有机遮罩：从太阳向右侧羽化消失 */
.mask-organ-zuo {
  -webkit-mask-image: radial-gradient(circle at left center, rgba(0,0,0,1) 40%, rgba(0,0,0,0) 100%);
  mask-image: radial-gradient(circle at left center, rgba(0,0,0,1) 40%, rgba(0,0,0,0) 100%);
}

/* 右翼有机遮罩：从太阳向左侧羽化消失 */
.mask-organ-you {
  -webkit-mask-image: radial-gradient(circle at right center, rgba(0,0,0,1) 40%, rgba(0,0,0,0) 100%);
  mask-image: radial-gradient(circle at right center, rgba(0,0,0,1) 40%, rgba(0,0,0,0) 100%);
}

/* 中间主图有机遮罩：双侧羽化，并定义向外渗透的“星状”核心区 */
.mask-organ-zhong {
  -webkit-mask-image: linear-gradient(to right, 
    rgba(0,0,0,0) 0%, 
    rgba(0,0,0,0.5) 15%, 
    rgba(0,0,0,1) 30%, 
    rgba(0,0,0,1) 70%, 
    rgba(0,0,0,0.5) 85%, 
    rgba(0,0,0,0) 100%
  );
  mask-image: linear-gradient(to right, 
    rgba(0,0,0,0) 0%, 
    rgba(0,0,0,0.5) 15%, 
    rgba(0,0,0,1) 30%, 
    rgba(0,0,0,1) 70%, 
    rgba(0,0,0,0.5) 85%, 
    rgba(0,0,0,0) 100%
  );
}

/* 超自然混合模式：让入侵图片以一种“滤色”方式叠加，创造光晕 */
.blend-invasion {
  mix-blend-mode: screen; /* 关键：滤色混合，保留亮部，隐藏暗部，创造融合光晕 */
}

/* 增强氛围层：在最上层加一个淡淡的径向暗角，统一全屏色调，压低拼接处的亮度 */
.ambiance-overlay-advanced {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
  pointer-events: none;
  background: radial-gradient(circle, rgba(0,0,0,0) 40%, rgba(0,0,0,0.4) 100%);
}
</style>