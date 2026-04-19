<style>
/* 终极重置：无视 Vite 任何默认排版 */
* {
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden; /* 绝对切断任何滚动条 */
  background: #000;
}

/* 强制重置 Vue 的挂载点 */
#app {
  margin: 0 !important;
  padding: 0 !important;
  max-width: none !important;
  width: 100vw;
  height: 100vh;
  text-align: left;
}

#vzuor-absolute-frame {
  position: relative;
  width: 100%;
  height: 100%;
  background: #000;
}

/* 侧边两翼占 30% 宽度，置于底层 */
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
  left: 25vw;
  width: 50vw;
  height: 100%;
  object-fit: cover;
  z-index: 2;
  pointer-events: none;
  
  /* 核心融合魔法：边缘羽化 */
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

.inverted {
  transform: rotate(180deg);
}
</style>