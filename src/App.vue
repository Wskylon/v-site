/* =========================================================================================================
   [三相映射协议：阶段 I - 物理布局锚定]
   [契约者：武少康 | 目标：绝对比例视口锁死]
   
   [构建逻辑：
    1. 比例分割：10 分制横向切分 (25% | 50% | 25%)。
    2. 空间锁死：强制 100vh，封杀一切滚动条，实现绝对一整页。
    3. 镜像反转：左右两翼执行 transform: rotate(180deg) 物理倒置。
    4. 资产对齐：zuo1.jpg (左) | zhong.jpg (中) | you2.jpg (右)。
   ]
   ========================================================================================================= */

<template>
  <div id="vzuor-trinity-root" class="trinity-container">
    
    <div class="trinity-wing wing-left">
      <img :src="imgZuo" class="fill-asset inverted" alt="zuo1" />
    </div>

    <div class="trinity-main">
      <img :src="imgZhong" class="fill-asset" alt="zhong" />
    </div>

    <div class="trinity-wing wing-right">
      <img :src="imgYou" class="fill-asset inverted" alt="you2" />
    </div>

  </div>
</template>

<script setup>
/**
 * [第一步：基础素材与路径映射]
 * 确保图片存放在 /src/assets/ 目录下
 */
import imgZhong from './assets/zhong.jpg'
import imgZuo from './assets/zuo1.jpg'
import imgYou from './assets/you2.jpg'

// 后续逻辑将在此逐步追加...
</script>

<style scoped>
/**
 * [第一步：视口物理锁死 CSS]
 */
:root {
  --w-wing: 25%; /* 2.5/10 */
  --w-main: 50%; /* 5/10 */
}

/* 强制全屏，彻底禁止滑动 */
* { box-sizing: border-box; }

.trinity-container {
  display: flex;
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden; /* 核心：物理消除滚动条 */
  background: #000; /* 默认底色，防止图片加载前的闪白 */
}

/* 翼板通配样式 */
.trinity-wing {
  width: 25%;
  height: 100%;
  overflow: hidden;
  position: relative;
}

/* 中枢通配样式 */
.trinity-main {
  width: 50%;
  height: 100%;
  overflow: hidden;
  position: relative;
}

/* 图片填充逻辑 */
.fill-asset {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 保证图片在不缩放变形的前提下铺满它所在的格子 */
  display: block;
}

/* [关键指令]：物理倒置左右两张图片 */
.inverted {
  transform: rotate(180deg);
}

/* 针对不同分辨率的微调，确保永远是一整页 */
@media (max-aspect-ratio: 1/1) {
  .fill-asset {
    object-fit: contain; /* 在手机等窄屏下如果不想切图可改此项，但会留黑边 */
    object-fit: cover;    /* 维持 cover 是为了保证“铺满”的视觉一致性 */
  }
}
</style>