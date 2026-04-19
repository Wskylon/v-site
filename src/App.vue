/* =========================================================================================================
   [三相映射协议：阶段 II - 绝对占满与视口锁死]
   [契约者：武少康 | 目标：消除白边，禁止位移，一进页面即满屏]
   ========================================================================================================= */

<template>
  <div id="vzuor-absolute-frame">
    
    <div class="col left-wing">
      <img :src="imgZuo" class="absolute-fill inverted" />
    </div>

    <div class="col center-main">
      <img :src="imgZhong" class="absolute-fill" />
    </div>

    <div class="col right-wing">
      <img :src="imgYou" class="absolute-fill inverted" />
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
}

/* 核心比例分配：2.5 : 5 : 2.5 */
.left-wing {
  flex: 2.5;
  background: #111; /* 底色防止加载闪烁 */
}

.center-main {
  flex: 5;
  background: #000;
}

.right-wing {
  flex: 2.5;
  background: #111;
}

.col {
  height: 100%;
  position: relative;
  overflow: hidden; /* 确保内容不会溢出格子 */
}

/* 强制占满算法：不再允许图片比容器小 */
.absolute-fill {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover; /* 自动裁切比例，保证物理上 100% 占满不留白 */
  display: block;
  pointer-events: none; /* 禁止鼠标对图片的任何拖拽干扰 */
}

/* 物理倒置 */
.inverted {
  transform: rotate(180deg);
}
</style>