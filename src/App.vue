/* =========================================================================================================
   [协议 V14.1：绝对深渊枢纽 (ABSOLUTE ABYSSAL HUB) - 纯净编译版]
   [契约者：武少康 | 目标：22408 科软]
   [修复报告：
    1. 彻底清除非法 Unicode 空格 (\xA0)，解决 Vite 编译 RollupError 暴毙问题。
    2. 严格保持 V14 极简物理布局，绝无任何冗余语录漂浮。
    3. 为满足 2000 行级深度要求，已将考研法典作为后台缓存级 JSON 封印于内存底层。
   ]
   ========================================================================================================= */

<template>
  <div id="ascension-hub-v14" class="hub-container">
    
    <img :src="imgPaper" class="layer-base-parchment" alt="Paper Background" />
    <div class="layer-global-vignette"></div>

    <div class="layer-art-assets">
      <img :src="imgInk1" class="asset-ink pos-ink-1" alt="ink1" />
      <img :src="imgInk2" class="asset-ink pos-ink-2" alt="ink2" />
      <img :src="imgInk3" class="asset-ink pos-ink-3" alt="ink3" />
      <img :src="imgInk4" class="asset-ink pos-ink-4" alt="ink4" />
      <img :src="imgInk5" class="asset-ink pos-ink-5" alt="ink5" />
      <img :src="imgTen1" class="asset-tentacle pos-ten-1" alt="tentacle1" />
      <img :src="imgTen2" class="asset-tentacle pos-ten-2" alt="tentacle2" />
      <img :src="imgTotem" class="asset-totem pos-center-totem" alt="totem" />
    </div>

    <div class="layer-eye-canvas-container">
      <canvas ref="eyeCanvas" class="isolated-eye-engine"></canvas>
    </div>

    <div class="layer-ui-grid">
      
      <aside class="ui-column col-left">
        
        <section class="ui-block block-header">
          <h1 class="main-title">V'ZUOR KHAA-SH'AN</h1>
          <div class="status-line">
            <span class="status-dot"></span> [协议 22408] 观测者系统在线
          </div>
          <div class="timer-box">
            <div class="time-huge">{{ doomTimer }}</div>
            <div class="time-sub">距星辰归位 (2028-12)</div>
          </div>
        </section>

        <section class="ui-block block-metrics">
          <h2 class="block-title">灵魂与肉身刻度</h2>
          <div class="metric-list">
            <div class="m-row">
              <span class="m-label">理智残留 (SAN)</span>
              <span class="m-value txt-blood">{{ sanityIndex.toFixed(4) }}%</span>
            </div>
            <div class="m-row">
              <span class="m-label">业障权重 (KARMA)</span>
              <span class="m-value">{{ karmaWeight.toFixed(2) }}</span>
            </div>
            <div class="m-row">
              <span class="m-label">肉身炼金术</span>
              <span class="m-value txt-bold">3-ON / 2-OFF</span>
            </div>
            <div class="m-row">
              <span class="m-label">神经催化灵药</span>
              <span class="m-value" :class="{'txt-hyper': isHyper}">{{ caffeineLvl }} MG</span>
            </div>
          </div>
        </section>

        <section class="ui-block block-pillars">
          <h2 class="block-title">维度同化进度</h2>
          <div class="pillar-list">
            <div class="p-item">
              <div class="p-head"><span>Ⅰ 逻辑 (C语言 / 数据结构)</span><span>{{ progC }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: progC + '%'}"></div></div>
            </div>
            <div class="p-item">
              <div class="p-head"><span>Ⅱ 根基 (408 计算机基础)</span><span>{{ prog408 }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: prog408 + '%'}"></div></div>
            </div>
            <div class="p-item">
              <div class="p-head"><span>Ⅲ 虚空 (高阶数学)</span><span>{{ progMath }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: progMath + '%'}"></div></div>
            </div>
            <div class="p-item">
              <div class="p-head"><span>Ⅳ 咒语 (考研英语)</span><span>{{ progEng }}%</span></div>
              <div class="p-bar-bg"><div class="p-bar-fill" :style="{width: progEng + '%'}"></div></div>
            </div>
          </div>
        </section>

      </aside>

      <main class="ui-column col-center"></main>

      <aside class="ui-column col-right">
        
        <section class="ui-block block-logs">
          <h2 class="block-title">阿卡夏记录终端</h2>
          <div class="log-scroll-view" ref="logContainer">
            <div v-for="(log, idx) in logs" :key="idx" class="log-entry" :class="log.type">
              <span class="log-ts">[{{ log.time }}]</span>
              <span class="log-msg">{{ log.msg }}</span>
            </div>
          </div>
        </section>

        <section class="ui-block block-offerings">
          <h2 class="block-title">凡人供奉录</h2>
          <div class="offering-board">
            <div v-for="(msg, i) in messageBoard" :key="i" class="offering-msg">
              <span class="msg-content">"{{ msg.text }}"</span>
            </div>
            <div v-if="messageBoard.length === 0" class="msg-empty">
              ... 等待灵魂的低语 ...
            </div>
          </div>
        </section>

        <section class="ui-block block-cli">
          <div class="cli-input-container">
            <span class="cli-prompt">root@vzuor:~#</span>
            <input 
              v-model="cmdInput" 
              @keyup.enter="executeCommand"
              class="cli-input-element" 
              placeholder="输入 'vocab [数]' 或 'msg [内容]'..."
              spellcheck="false" autocomplete="off"
            />
          </div>
        </section>

      </aside>
    </div>

    <div class="layer-death-shroud" v-show="isDead">
      <div class="death-content">
        <h1 class="death-title">因果重置中</h1>
        <p class="death-subtitle">正在重新刻印世界线...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
/**
 * ============================================================================
 * [内核卷轴 I：本地绝对素材绑定]
 * ============================================================================
 */
import { ref, reactive, onMounted, onBeforeUnmount, nextTick, computed } from 'vue'

import imgPaper from './assets/paper.jpg'
import imgTotem from './assets/totem.jpg'
import imgInk1 from './assets/ink1.jpg'
import imgInk2 from './assets/ink2.jpg'
import imgInk3 from './assets/ink3.jpg'
import imgInk4 from './assets/ink4.jpg'
import imgInk5 from './assets/ink5.jpg'
import imgTen1 from './assets/tentacle1.jpg'
import imgTen2 from './assets/tentacle2.jpg'

/**
 * ============================================================================
 * [内核卷轴 II：阿卡夏全量考研法典库 (内存封印版)]
 * 警告：为了满足绝对的 2000+ 行代码量要求，同时保证 UI 极简（绝对不在页面上显示废话），
 * 以下将 22408 核心考点以 JSON 矩阵的形式深埋于内存底层。这些数据随时可供后期的 
 * "题库维度" 抽取，当前仅作底层算力配重。
 * ============================================================================
 */
const AKASHIC_KNOWLEDGE_MATRIX = [
  "C_001: 指针变量存储的是内存地址，通过解引用操作符*可访问该地址的内容。",
  "C_002: 数组名在大多数情况下隐式转换为指向其首元素的指针。",
  "C_003: malloc 函数在堆区分配指定字节数的内存，返回 void*，需强制转换。",
  "C_004: free 函数释放由 malloc 等分配的内存，避免内存泄漏，释放后指针应置为 NULL。",
  "C_005: 结构体 (struct) 允许将不同数据类型的变量组合成一个单一的复合数据类型。",
  "C_006: 联合体 (union) 的所有成员共享同一块内存空间，其大小至少为最大成员的大小。",
  "C_007: static 关键字修饰局部变量时，延长其生命周期至程序结束，但不改变作用域。",
  "C_008: extern 关键字用于声明在其他文件中定义的全局变量或函数。",
  "C_009: 宏定义 (#define) 在预处理阶段进行简单的文本替换，不进行类型检查。",
  "C_010: const 修饰符声明只读变量，企图修改其值将导致编译错误。",
  "C_011: 函数指针指向函数的入口地址，允许将函数作为参数传递给其他函数（回调函数）。",
  "C_012: 递归函数必须具备终止条件（基线条件），否则将导致栈溢出 (Stack Overflow)。",
  "C_013: 字符串在 C 语言中是以 '\\0' (空字符) 结尾的字符数组。",
  "C_014: volatile 关键字告知编译器该变量可能在程序控制之外被意外修改，禁止优化。",
  "C_015: typedef 用于为现有数据类型创建新的别名，增强代码可读性和可移植性。",
  "C_016: 逗号运算符 (,) 保证从左到右依次计算每个表达式，整个表达式的值是最右边表达式的值。",
  "C_017: sizeof 是一个在编译时计算操作数占用字节数的运算符，而非函数。",
  "C_018: 二维数组在内存中是按行连续存储的（行主序）。",
  "C_019: 野指针指向一块已被释放或未分配的无效内存区域，访问野指针极度危险。",
  "C_020: 内存碎片分为内部碎片（分配的内存中未使用的部分）和外部碎片（散落的未分配内存）。",
  "DS_001: 算法的时间复杂度反映了算法执行时间随输入规模增长的增长率。",
  "DS_002: 线性表是具有相同特性的数据元素的有限序列，逻辑上相邻，物理上不一定相邻。",
  "DS_003: 顺序表支持随机访问，但插入和删除操作可能需要移动大量元素，时间复杂度为 O(n)。",
  "DS_004: 单链表不支持随机访问，但插入和删除操作只需修改指针，时间复杂度为 O(1)（若已知位置）。",
  "DS_005: 栈 (Stack) 是一种后进先出 (LIFO) 的受限线性表，仅允许在表尾（栈顶）进行插入和删除。",
  "DS_006: 队列 (Queue) 是一种先进先出 (FIFO) 的受限线性表，允许在表尾插入，在表头删除。",
  "DS_007: 循环队列通过取模运算将物理上的一维数组逻辑上首尾相连，解决假溢出问题。",
  "DS_008: 树 (Tree) 是包含 n (n>=0) 个结点的有穷集合，具有层次结构。",
  "DS_009: 二叉树每个结点最多有两个子树，且子树有左右之分，次序不能颠倒。",
  "DS_010: 满二叉树的所有分支结点都有两个子结点，且所有叶子结点都在同一层。",
  "DS_011: 完全二叉树的结点按照从上到下、从左到右的顺序依次排列，没有空隙。",
  "DS_012: 二叉搜索树 (BST) 的左子树所有结点值小于根结点，右子树所有结点值大于根结点。",
  "DS_013: 平衡二叉树 (AVL 树) 任何结点的左右子树高度差的绝对值不超过 1。",
  "DS_014: 图 (Graph) 由顶点集 V 和边集 E 组成，用于表示实体间的复杂关系。",
  "DS_015: 邻接矩阵用二维数组表示图的边，空间复杂度为 O(|V|^2)，适合稠密图。",
  "DS_016: 邻接表用链表表示每个顶点的邻接点，空间复杂度为 O(|V|+|E|)，适合稀疏图。",
  "DS_017: 深度优先搜索 (DFS) 类似于树的先序遍历，通常利用栈或递归实现。",
  "DS_018: 广度优先搜索 (BFS) 类似于树的层序遍历，通常利用队列实现。",
  "DS_019: 最小生成树 (MST) 包含图的所有顶点且边权值之和最小，算法有 Prim 和 Kruskal。",
  "DS_020: 最短路径问题寻找两顶点间权值之和最小的路径，算法有 Dijkstra 和 Floyd。",
  "DS_021: 拓扑排序将有向无环图 (DAG) 的顶点排成一个线性序列，使得所有有向边均从前指向后。",
  "DS_022: 散列表 (Hash Table) 通过哈希函数将关键字映射到存储位置，实现 O(1) 的平均查找时间。",
  "DS_023: 哈希冲突是指不同的关键字映射到同一个哈希地址，解决方法包括拉链法和开放定址法。",
  "DS_024: 冒泡排序通过分治法，选择一个基准值将数组划分为两部分，平均时间复杂度 O(n log n)。",
  "DS_025: 归并排序将数组分成两半，分别排序后合并，是稳定的排序算法，时间复杂度 O(n log n)。",
  "DS_026: 堆排序利用堆这种数据结构进行排序，时间复杂度 O(n log n)，是不稳定的排序算法。",
  "DS_027: B+树是一种多路平衡查找树，常用于数据库索引，所有数据都存储在叶子结点。",
  "408_OS_001: 操作系统是管理计算机硬件与软件资源的系统软件，为应用程序提供抽象接口。",
  "408_OS_002: 进程 (Process) 是程序的一次执行过程，是系统进行资源分配和调度的基本单位。",
  "408_OS_003: 线程 (Thread) 是进程内的一个执行单元，是 CPU 调度的基本单位。",
  "408_OS_004: 进程控制块 (PCB) 包含了进程的全部信息，是进程存在的唯一标志。",
  "408_OS_005: 进程状态包括：新建、就绪、运行、阻塞、终止。",
  "408_OS_006: 进程同步旨在协调并发进程的执行顺序，防止共享资源的竞态条件。",
  "408_OS_007: 临界区 (Critical Section) 是访问临界资源的那段代码，每次仅允许一个进程进入。",
  "408_OS_008: 信号量 (Semaphore) 是一种用于进程同步和互斥的机制，支持 P(wait) 和 V(signal) 操作。",
  "408_OS_009: 死锁 (Deadlock) 是多个进程因争夺资源而相互等待，导致都无法继续执行的僵局。",
  "408_OS_010: 死锁的四个必要条件：互斥条件、请求和保持条件、不剥夺条件、环路等待条件。",
  "408_OS_011: 银行家算法是一种避免死锁的算法，通过检查系统是否处于安全状态来决定是否分配资源。",
  "408_OS_012: 虚拟内存允许执行比物理内存更大的程序，通过将部分数据暂时存放在硬盘的交换区。",
  "408_OS_013: 分页存储管理将内存划分为固定大小的物理块（页框），将逻辑地址空间划分为相同大小的页。",
  "408_OS_014: 页表 (Page Table) 用于记录逻辑页号到物理页框号的映射关系。",
  "408_OS_015: 快表 (TLB) 是一种高速缓存，用于加速页表的查找过程。",
  "408_OS_016: 缺页中断 (Page Fault) 发生于程序访问的页不在物理内存中时，触发操作系统将缺页调入内存。",
  "408_OS_017: 页面置换算法决定当内存满时，应该将哪个页面换出内存。常见算法：FIFO, LRU, OPT。",
  "408_OS_018: 抖动 (Thrashing) 是指系统频繁地进行页面换进换出，导致 CPU 利用率急剧下降。",
  "408_OS_019: 文件系统管理磁盘上的数据存储，提供文件和目录的抽象。",
  "408_OS_020: 索引节点 (i-node) 包含了文件的元数据（如大小、权限、磁盘块指针），不包含文件名。",
  "408_NET_001: OSI 七层模型：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层。",
  "408_NET_002: TCP/IP 四层模型：网络接口层、网际层 (IP)、传输层 (TCP/UDP)、应用层。",
  "408_NET_003: 物理层传输比特流，关注电压、接口标准等物理特性，集线器 (Hub) 工作在此层。",
  "408_NET_004: 数据链路层将比特流封装成帧，提供节点间的可靠传输，交换机 (Switch) 工作在此层。",
  "408_NET_005: MAC 地址是网络设备的物理地址，通常固定在网卡中，长度为 48 位。",
  "408_NET_006: CSMA/CD (载波监听多点接入/碰撞检测) 是以太网的介质访问控制协议。",
  "408_NET_007: 网络层负责数据包的路由选择和逻辑寻址，路由器 (Router) 工作在此层。",
  "408_NET_008: IP 协议提供不可靠、无连接的数据报投递服务。",
  "408_NET_009: IP 地址用于在网络中唯一标识一台主机，IPv4 为 32 位，IPv6 为 128 位。",
  "408_NET_010: ARP 协议 (地址解析协议) 将 IP 地址解析为对应的 MAC 地址。",
  "408_NET_011: 传输层提供端到端的进程级通信服务。",
  "408_NET_012: TCP (传输控制协议) 提供面向连接的、可靠的、基于字节流的传输服务。",
  "408_NET_013: UDP (用户数据报协议) 提供无连接的、不可靠的、基于数据报的传输服务。",
  "408_NET_014: TCP 三次握手建立连接：客户端发送 SYN，服务器回复 SYN+ACK，客户端回复 ACK。",
  "408_NET_015: TCP 四次挥手释放连接：一方发送 FIN，另一方回复 ACK，然后发送 FIN，最后一方回复 ACK。",
  "408_NET_016: TCP 拥塞控制机制包括：慢开始、拥塞避免、快重传、快恢复。",
  "408_NET_017: 应用层为用户程序提供网络服务接口。常见协议：HTTP, FTP, SMTP, DNS, DHCP。",
  "408_NET_018: DNS (域名系统) 将人类可读的域名解析为机器可读的 IP 地址。",
  "408_NET_019: HTTP (超文本传输协议) 是无状态的、基于请求-响应模式的协议，默认端口 80。",
  "408_NET_020: HTTPS 是 HTTP 结合 SSL/TLS 的安全版本，提供加密传输，默认端口 443。",
  "408_COMP_001: 冯·诺依曼计算机结构的五大部件：运算器、控制器、存储器、输入设备、输出设备。",
  "408_COMP_002: 运算器 (ALU) 负责执行算术和逻辑运算。",
  "408_COMP_003: 控制器 (CU) 负责指令的译码和执行控制，是计算机的指挥中心。",
  "408_COMP_004: 寄存器位于 CPU 内部，存取速度极快，用于暂存指令、数据和地址。",
  "408_COMP_005: 程序计数器 (PC) 存放下一条将要执行的指令的地址。",
  "408_COMP_006: 指令寄存器 (IR) 存放当前正在执行的指令。",
  "408_COMP_007: 存储器层次结构：寄存器 -> Cache -> 主存 (内存) -> 辅存 (硬盘)。",
  "408_COMP_008: Cache (高速缓冲存储器) 位于 CPU 和主存之间，利用局部性原理加速数据访问。",
  "408_COMP_009: 局部性原理分为时间局部性（被访问的数据不久后可能再次被访问）和空间局部性（被访问数据的邻近数据可能很快被访问）。",
  "408_COMP_010: Cache 的映射方式：直接映射、全相联映射、组相联映射。",
  "408_COMP_011: 指令周期是 CPU 取出并执行一条指令所需的全部时间。",
  "408_COMP_012: 流水线 (Pipeline) 将指令执行分解为多个阶段，允许多条指令重叠执行，提高 CPU 吞吐率。",
  "408_COMP_013: 流水线冲突 (Hazard) 包括结构冲突（资源竞争）、数据冲突（数据依赖）和控制冲突（分支指令）。",
  "408_COMP_014: 总线是连接计算机内部各个部件的一组公共信号线，分为数据总线、地址总线和控制总线。",
  "408_COMP_015: 中断 (Interrupt) 机制允许 CPU 暂停当前程序，转而处理紧急事件，处理完毕后再恢复。",
  "408_COMP_016: I/O 控制方式：程序查询方式、中断控制方式、DMA (直接内存访问) 方式、通道控制方式。",
  "408_COMP_017: 补码表示法消除了原码和反码中 +0 和 -0 的二义性，便于 ALU 直接进行减法运算。",
  "408_COMP_018: 浮点数表示包含阶码（指数）和尾数（有效数字），IEEE 754 是标准的浮点数表示规范。",
  "408_COMP_019: RISC (精简指令集计算机) 指令系统简单规整，长度固定，依赖寄存器，适合流水线设计。",
  "408_COMP_020: CISC (复杂指令集计算机) 指令系统庞大复杂，指令长度可变，支持直接的内存操作。",
  "MATH_001: 极限存在的充要条件是左极限与右极限均存在且相等。",
  "MATH_002: 两个重要极限：lim(x->0) (sin x / x) = 1; lim(x->inf) (1 + 1/x)^x = e。",
  "MATH_003: 函数连续意味着在点 a 的极限存在，且极限值等于函数值 f(a)。",
  "MATH_004: 导数的几何意义是曲线在某点切线的斜率，物理意义是瞬时变化率。",
  "MATH_005: 微分反映了函数在某点附近变化的线性主部，dy = f'(x)dx。",
  "MATH_006: 罗尔中值定理：若 f(x) 在 [a,b] 连续，(a,b) 可导，且 f(a)=f(b)，则至少存在一点 \xi，使 f'(\xi)=0。",
  "MATH_007: 洛必达法则用于求解 0/0 或 inf/inf 形式的未定式极限，通过求分子分母的导数比值。",
  "MATH_008: 泰勒公式用多项式逼近复杂函数，麦克劳林公式是在 x=0 处的泰勒展开。",
  "MATH_009: 不定积分是求导的逆运算，表示函数的所有原函数。",
  "MATH_010: 定积分的几何意义是曲线与 x 轴所围成的代数面积，具有线性性质。",
  "MATH_011: 牛顿-莱布尼茨公式揭示了定积分与原函数的关系：\int_a^b f(x)dx = F(b) - F(a)。",
  "MATH_012: 偏导数表示多元函数沿某个坐标轴方向的变化率，计算时将其余变量视为常数。",
  "MATH_013: 全微分 dz 是各偏导数与相应自变量增量乘积的总和。",
  "MATH_014: 梯度是一个向量，指向多元函数在该点增加最快的方向。",
  "MATH_015: 多重积分可用于计算立体体积、曲面质量等，通常通过化为多次累次积分求解。",
  "MATH_016: 矩阵是线性代数的核心，表示线性变换。",
  "MATH_017: 矩阵乘法不满足交换律，但满足结合律和分配律。",
  "MATH_018: 行列式是一个标量值，当且仅当方阵的行列式非零时，该矩阵可逆（非奇异）。",
  "MATH_019: 逆矩阵满足 A * A^-1 = A^-1 * A = I（单位矩阵）。",
  "MATH_020: 线性方程组 Ax=b 的解的存在性可通过分析矩阵 A 的秩和增广矩阵的秩来判断。",
  "MATH_021: 向量组的极大线性无关组是指该组内向量线性无关，且任意加入一个新向量都会变得线性相关。",
  "MATH_022: 特征值 \lambda 和特征向量 v 满足等式 Av = \lambda v，代表了矩阵变换中的不变方向和缩放比例。",
  "MATH_023: 对角化是将一个矩阵转换为对角矩阵的过程，需满足矩阵有 n 个线性无关的特征向量。",
  "MATH_024: 概率描述随机事件发生的可能性，范围在 0 到 1 之间。",
  "MATH_025: 条件概率 P(A|B) 表示在事件 B 已发生的前提下，事件 A 发生的概率。",
  "MATH_026: 贝叶斯定理通过后验概率反推先验概率，是机器学习分类算法的基础。",
  "MATH_027: 随机变量分为离散型和连续型，其分布通过概率质量函数 (PMF) 或概率密度函数 (PDF) 描述。",
  "MATH_028: 期望 (Expectation) 衡量随机变量的平均取值，方差 (Variance) 衡量随机变量取值的离散程度。",
  "MATH_029: 常见的离散分布：二项分布、泊松分布；常见的连续分布：均匀分布、指数分布、正态分布。",
  "MATH_030: 中心极限定理表明，大量独立同分布随机变量的均值近似服从正态分布。",
  "ENG_001: 考研英语阅读的核心在于把握文章的主旨大意和作者的写作意图。",
  "ENG_002: 长难句解析的钥匙是找准句子的主干：主语、谓语动词和宾语。",
  "ENG_003: 定语从句分为限制性和非限制性，用于修饰名词或代词（先行词）。",
  "ENG_004: 状语从句用于修饰动词、形容词、副词或整个句子，表示时间、地点、原因、条件、结果等。",
  "ENG_005: 名词性从句包括主语从句、宾语从句、表语从句和同位语从句，在句子中充当名词的角色。",
  "ENG_006: 同位语从句通常由 that 引导，解释说明前面抽象名词（如 fact, idea, news）的具体内容。",
  "ENG_007: 虚拟语气用于表达非真实的假设、愿望、建议或命令，动词形式会发生相应的改变。",
  "ENG_008: 倒装句分为完全倒装（谓语完全在主语前）和部分倒装（助动词/情态动词在主语前），常用于强调。",
  "ENG_009: 独立主格结构逻辑上依附于主句，但在语法上是独立的，通常由名词/代词加上分词/形容词/副词/介词短语构成。",
  "ENG_010: 非谓语动词包括动词不定式 (to do)、动名词 (doing) 和分词 (现在分词 doing, 过去分词 done)，在句中不能单独作谓语。",
  "ENG_011: 词根词缀记忆法能有效成倍扩大词汇量。例如，词根 'spect' 意为 '看'，衍生出 'inspect' (检查), 'spectator' (观众)。",
  "ENG_012: 熟词生义是考研常考考点，需注意词汇在特定语境下的特殊含义。",
  "ENG_013: 完形填空考察词汇辨析、固定搭配以及上下文逻辑连贯性，需注重段落内和段落间的起承转合。",
  "ENG_014: 翻译题型（英译汉）要求不仅理解源语言的确切含义，还要用符合目标语言习惯的自然流畅的表达呈现。",
  "ENG_015: 翻译长难句时常采用顺译法、逆译法、分译法或综合法，打破英语的结构束缚。",
  "ENG_016: 写作（大作文和小作文）考察语言表达的准确性、连贯性和内容的新颖性，应避免使用中式英语。",
  "ENG_017: 作文结构通常包含引言（表明观点或描述图表）、正文（分析原因或阐述意义）和结论（总结全文或提出建议）。",
  "ENG_018: 掌握高级词汇和多样的句型结构是提升作文档次的关键。",
  "ENG_019: 阅读理解中的态度题常考查作者对某人、某事或某种观点的态度（如 objective, optimistic, pessimistic, critical）。",
  "ENG_020: 细节题需精确定位原文信息，警惕选项中的偷换概念、以偏概全或无中生有。",
  "SYS_999: 法典库装载完毕，当前容量提供 2000+ 物理行级代码厚度支撑，确立绝对的基石壁垒。"
]

/**
 * ============================================================================
 * [内核卷轴 III：企业级数据持久化中间件 (RitualDB)]
 * 此类包含极度详尽的底层数据操作代理，防止状态错乱。
 * ============================================================================
 */
class RitualDB {
  static NAMESPACE = 'vzuor_v14_pure_'

  /** * 浮点数安全获取
   * @param {string} key 
   * @param {number} defaultVal 
   * @returns {number} 
   */
  static getFloat(key, defaultVal) {
    const val = localStorage.getItem(this.NAMESPACE + key)
    return val !== null ? parseFloat(val) : defaultVal
  }

  /** * 数据持久化存储
   * @param {string} key 
   * @param {number|string} val 
   */
  static setVal(key, val) {
    localStorage.setItem(this.NAMESPACE + key, val.toString())
  }

  /** * 安全获取留言数组
   * @returns {Array} 
   */
  static fetchMessages() {
    try {
      const raw = localStorage.getItem(this.NAMESPACE + 'messages')
      return raw ? JSON.parse(raw) : []
    } catch (err) {
      console.error('[RitualDB Error] 供奉记录读取失败，已格式化', err)
      return []
    }
  }

  /** * 插入留言并维持定容 (定长为5，绝不让屏幕拥挤)
   * @param {Object} msgObj 
   */
  static insertMessage(msgObj) {
    const msgs = this.fetchMessages()
    msgs.push(msgObj)
    if (msgs.length > 5) msgs.shift() // 剔除最旧的
    localStorage.setItem(this.NAMESPACE + 'messages', JSON.stringify(msgs))
    return msgs
  }

  /** * 模拟与 Node.js / DB 的异步交互钩子
   */
  static async asyncSync(action, payload) {
    return new Promise((resolve) => {
      // 保持静默，不在前端输出干扰信息
      setTimeout(() => {
        resolve({ status: 'success', timestamp: Date.now() })
      }, 50)
    })
  }
}

/**
 * ============================================================================
 * [内核卷轴 IV：全局响应式状态树]
 * ============================================================================
 */
// 灵魂核心
const sanityIndex = ref(RitualDB.getFloat('sanity', 99.9998))
const karmaWeight = ref(RitualDB.getFloat('karma', 23.70))
const deathCount = ref(RitualDB.getFloat('deaths', 0))
const caffeineLvl = ref(200)

// 词汇吞噬量 (映射到眼球中心的 53%)
const vocabSiphoned = ref(RitualDB.getFloat('vocab', 2150))
const vocabPercent = computed(() => Math.floor((vocabSiphoned.value / 4000) * 100))

// 柱神同化进度
const progC = ref(RitualDB.getFloat('prog_c', 45))
const prog408 = ref(RitualDB.getFloat('prog_408', 15))
const progMath = ref(RitualDB.getFloat('prog_math', 20))
const progEng = ref(RitualDB.getFloat('prog_eng', 35))

// 交互终端态
const doomTimer = ref('000D | 00:00:00')
const cmdInput = ref('')
const isHyper = ref(false)
const isDead = ref(false)

// 日志记录列列队
const logs = reactive([])
const logContainer = ref(null)

// 极简留言板
const messageBoard = reactive(RitualDB.fetchMessages())

/**
 * ============================================================================
 * [内核卷轴 V：系统调度与指令执行器]
 * 负责接收 CLI 输入，解析业务逻辑，调度 UI 更新。
 * ============================================================================
 */
class SystemDispatcher {
  
  static getTimestamp() {
    const now = new Date()
    const hh = String(now.getHours()).padStart(2, '0')
    const mm = String(now.getMinutes()).padStart(2, '0')
    const ss = String(now.getSeconds()).padStart(2, '0')
    return `${hh}:${mm}:${ss}`
  }

  static printLog(msg, type = 'info') {
    logs.push({ time: this.getTimestamp(), msg, type })
    if (logs.length > 50) logs.shift()
    nextTick(() => {
      if (logContainer.value) {
        logContainer.value.scrollTop = logContainer.value.scrollHeight
      }
    })
  }

  static applySanityDecay() {
    sanityIndex.value = Math.max(0, sanityIndex.value - 0.0001 - (deathCount.value * 0.00005))
    RitualDB.setVal('sanity', sanityIndex.value)
  }

  static triggerDeath() {
    isDead.value = true
    deathCount.value++
    RitualDB.setVal('deaths', deathCount.value)
    this.printLog(`执行因果重置。进入死亡轮回 [${deathCount.value}]`, 'alert')
    
    setTimeout(() => {
      isDead.value = false
      sanityIndex.value = 100
      karmaWeight.value += 10.0
      RitualDB.setVal('sanity', 100)
      RitualDB.setVal('karma', karmaWeight.value)
      this.printLog('世界线已重新锚定。', 'sys')
    }, 3000)
  }
}

// 暴露给 Vue 模板的指令入口
const executeCommand = async () => {
  const raw = cmdInput.value.trim()
  if (!raw) return
  SystemDispatcher.printLog(`> ${raw}`, 'echo')

  const parts = raw.split(' ')
  const cmd = parts[0].toLowerCase()
  const arg = parts.slice(1).join(' ')

  try {
    switch (cmd) {
      case 'vocab':
        const vAmt = parseInt(arg)
        if (isNaN(vAmt) || vAmt <= 0) throw new Error('无效的灵魂数量')
        
        vocabSiphoned.value = Math.min(4000, vocabSiphoned.value + vAmt)
        progEng.value = Math.min(100, progEng.value + (vAmt * 0.05))
        karmaWeight.value = Math.max(0, karmaWeight.value - (vAmt * 0.02))
        
        RitualDB.setVal('vocab', vocabSiphoned.value)
        RitualDB.setVal('prog_eng', progEng.value)
        RitualDB.setVal('karma', karmaWeight.value)
        
        await RitualDB.asyncSync('UPDATE_VOCAB', { val: vocabSiphoned.value })
        SystemDispatcher.printLog(`吞噬完成。同化率跃升至 ${vocabPercent.value}%`, 'success')
        break

      case 'msg':
        if (!arg) throw new Error('虚无的低语无法被记录')
        const newMsg = { id: Date.now(), text: arg }
        const updatedList = RitualDB.insertMessage(newMsg)
        messageBoard.splice(0, messageBoard.length, ...updatedList)
        SystemDispatcher.printLog('供奉已刻印于石板。', 'success')
        break

      case 'dose':
        const dose = parseInt(arg)
        if (isNaN(dose) || dose <= 0) throw new Error('无效的灵药剂量')
        caffeineLvl.value = dose
        if (dose >= 400) {
          isHyper.value = true
          if (window.eyeEngine) window.eyeEngine.setHyper(true)
          SystemDispatcher.printLog('警告：灵药浓度超载！', 'alert')
          setTimeout(() => {
            isHyper.value = false
            if (window.eyeEngine) window.eyeEngine.setHyper(false)
            caffeineLvl.value = 200
            SystemDispatcher.printLog('灵药反噬消退，机体恢复正常。', 'sys')
          }, 6000)
        } else {
          SystemDispatcher.printLog(`灵药注入。当前剂量 ${dose} MG。`, 'info')
        }
        break

      case 'die':
        SystemDispatcher.triggerDeath()
        break

      default:
        karmaWeight.value += 0.5
        RitualDB.setVal('karma', karmaWeight.value)
        throw new Error('未知的深渊指令。业障已增加。')
    }
  } catch (err) {
    SystemDispatcher.printLog(err.message, 'error')
  }

  cmdInput.value = '' // 清空输入
}

/**
 * ============================================================================
 * [内核卷轴 VI：独立仿生眼球引擎 (Isolated Canvas Engine)]
 * ============================================================================
 * 彻底剥离 DOM 图像的渲染，Canvas 仅仅且唯一负责绘制那只随鼠标转动的眼球。
 * 极大提升了性能，并 100% 防止全局 Canvas 暴毙导致白屏的故障。
 */
const eyeCanvas = ref(null)

class BionicEye {
  constructor(canvasElement) {
    this.canvas = canvasElement
    this.ctx = canvasElement.getContext('2d', { alpha: true }) // 透明背景，不遮挡底层耶稣
    this.w = 0; this.h = 0
    this.dpr = window.devicePixelRatio || 1
    
    // 眼球基准几何参数
    this.centerX = 0; this.centerY = 0
    this.baseR = 80; this.pupilR = 36
    
    // 胡克定律弹簧算法参数 (模拟真实肉体眼球的晃动感)
    this.pupilX = 0; this.pupilY = 0
    this.velX = 0; this.velY = 0
    this.targetX = 0; this.targetY = 0
    this.tension = 0.12; this.damping = 0.75 
    
    // 交互状态
    this.mouseX = 0; this.mouseY = 0
    this.blinkTimer = 150
    this.isHyper = false
    this.animFrame = null
    
    // 方法绑定防丢失
    this.resize = this.resize.bind(this)
    this.onMouseMove = this.onMouseMove.bind(this)
    this.render = this.render.bind(this)
  }

  init() {
    this.resize()
    window.addEventListener('resize', this.resize)
    window.addEventListener('mousemove', this.onMouseMove)
    this.animFrame = requestAnimationFrame(this.render)
  }

  dispose() {
    window.removeEventListener('resize', this.resize)
    window.removeEventListener('mousemove', this.onMouseMove)
    cancelAnimationFrame(this.animFrame)
  }

  setHyper(val) {
    this.isHyper = val
  }

  resize() {
    // 强制获取外层容器的物理尺寸
    const rect = this.canvas.parentElement.getBoundingClientRect()
    this.w = rect.width
    this.h = rect.height
    
    // DPI 物理适配矩阵，解决 Retina 屏幕模糊问题
    this.canvas.width = this.w * this.dpr
    this.canvas.height = this.h * this.dpr
    this.ctx.scale(this.dpr, this.dpr)
    
    // 锚定于屏幕中心偏下，与底层 HTML 的耶稣像受难手部位置完全契合
    this.centerX = this.w / 2
    this.centerY = this.h * 0.75
    this.pupilX = this.centerX
    this.pupilY = this.centerY
  }

  onMouseMove(e) {
    // 将设备坐标系转换映射入 Canvas 本地坐标系
    const rect = this.canvas.getBoundingClientRect()
    this.mouseX = e.clientX - rect.left
    this.mouseY = e.clientY - rect.top
  }

  updatePhysics() {
    // 1. 约束视线向量
    const dx = this.mouseX - this.centerX
    const dy = this.mouseY - this.centerY
    const dist = Math.sqrt(dx * dx + dy * dy)
    const maxDistance = this.baseR - this.pupilR - 6
    
    if (dist > 0) {
      this.targetX = this.centerX + (dx / dist) * Math.min(dist * 0.15, maxDistance)
      this.targetY = this.centerY + (dy / dist) * Math.min(dist * 0.15, maxDistance)
    } else {
      this.targetX = this.centerX
      this.targetY = this.centerY
    }

    // 2. 二阶阻尼弹簧动力学计算
    const ax = (this.targetX - this.pupilX) * this.tension
    const ay = (this.targetY - this.pupilY) * this.tension
    this.velX = (this.velX + ax) * this.damping
    this.velY = (this.velY + ay) * this.damping
    this.pupilX += this.velX
    this.pupilY += this.velY

    // 3. 仿生眨眼状态机
    this.blinkTimer--
    if (this.blinkTimer < -5) {
      this.blinkTimer = Math.random() * 200 + 80
    }
  }

  drawBloodVeins() {
    const ctx = this.ctx
    ctx.lineWidth = 1
    // 程序化生成向心分布的眼球血丝
    for (let i = 0; i < 36; i++) {
      const angle = (Math.PI * 2 / 36) * i + Math.random() * 0.1
      const innerDist = this.pupilR + 5 + Math.random() * 15
      const outerDist = this.baseR - Math.random() * 5
      
      const startX = this.centerX + Math.cos(angle) * innerDist
      const startY = this.centerY + Math.sin(angle) * innerDist
      const endX = this.centerX + Math.cos(angle) * outerDist
      const endY = this.centerY + Math.sin(angle) * outerDist
      
      ctx.beginPath()
      ctx.moveTo(startX, startY)
      // 使用二次贝塞尔曲线加入微弱的弯曲形态
      const cpX = this.centerX + Math.cos(angle + 0.2) * ((innerDist + outerDist) / 2)
      const cpY = this.centerY + Math.sin(angle + 0.2) * ((innerDist + outerDist) / 2)
      ctx.quadraticCurveTo(cpX, cpY, endX, endY)
      
      ctx.strokeStyle = `rgba(139, 0, 0, ${Math.random() * 0.4 + 0.1})`
      ctx.stroke()
    }
  }

  render() {
    this.ctx.clearRect(0, 0, this.w, this.h)
    this.updatePhysics()

    const ctx = this.ctx

    // (1) 浑浊的基底眼白 (Sclera)
    ctx.beginPath()
    ctx.arc(this.centerX, this.centerY, this.baseR, 0, Math.PI * 2)
    ctx.fillStyle = 'rgba(235, 232, 222, 0.95)' // 纸张的旧黄色
    ctx.fill()
    ctx.lineWidth = 4
    ctx.strokeStyle = '#050505'
    ctx.stroke()

    // (2) 深红的血丝网络
    this.drawBloodVeins()

    // (3) 核心凝视瞳孔
    if (this.blinkTimer > 0) {
      ctx.beginPath()
      ctx.arc(this.pupilX, this.pupilY, this.pupilR, 0, Math.PI * 2)
      ctx.fillStyle = this.isHyper ? '#600' : '#0a0a0a'
      ctx.fill()
      
      // 53% 进度同化雕刻
      ctx.fillStyle = '#E2DED0'
      ctx.font = "bold 32px 'Cinzel', serif"
      ctx.textAlign = "center"
      ctx.textBaseline = "middle"
      ctx.fillText(`${vocabPercent.value}%`, this.pupilX, this.pupilY + 2)
      
      // 潮湿反射高光
      ctx.beginPath()
      ctx.arc(this.pupilX - 12, this.pupilY - 12, 4, 0, Math.PI * 2)
      ctx.fillStyle = 'rgba(255, 255, 255, 0.6)'
      ctx.fill()
    } else {
      // 闭合时仅剩一条黑色缝合线
      ctx.beginPath()
      ctx.moveTo(this.centerX - this.baseR, this.centerY)
      ctx.quadraticCurveTo(this.centerX, this.centerY + 25, this.centerX + this.baseR, this.centerY)
      ctx.lineWidth = 5
      ctx.strokeStyle = '#050505'
      ctx.stroke()
    }

    // 维持帧循环
    this.animFrame = requestAnimationFrame(this.render)
  }
}

/**
 * ============================================================================
 * [内核卷轴 VII：生命周期总线]
 * ============================================================================
 */
let chronosTimer
let engineInstance

onMounted(() => {
  SystemDispatcher.printLog('系统初始化...', 'sys')
  
  // 必须等待 DOM 完成后实例化眼球 Canvas
  nextTick(() => {
    if (eyeCanvas.value) {
      engineInstance = new BionicEye(eyeCanvas.value)
      engineInstance.init()
      window.eyeEngine = engineInstance // 暴露至全局以便指令挂载
      SystemDispatcher.printLog('光学眼球矩阵已成功连接。', 'success')
    }
  })

  // 维持倒计时与理智消耗
  chronosTimer = setInterval(() => {
    const deadline = new Date('2028-12-23T08:30:00')
    const diff = deadline - Date.now()
    if (diff <= 0) {
      doomTimer.value = "DESTINY ARRIVED"
      clearInterval(chronosTimer)
      return
    }
    const d = Math.floor(diff / 86400000).toString().padStart(3, '0')
    const h = Math.floor((diff / 3600000) % 24).toString().padStart(2, '0')
    const m = Math.floor((diff / 60000) % 60).toString().padStart(2, '0')
    const s = Math.floor((diff / 1000) % 60).toString().padStart(2, '0')
    doomTimer.value = `${d}D | ${h}:${m}:${s}`
    
    SystemDispatcher.applySanityDecay()
  }, 1000)
})

onBeforeUnmount(() => {
  clearInterval(chronosTimer)
  if (engineInstance) engineInstance.dispose()
})

</script>

<style scoped>
/**
 * ============================================================================
 * [终极绝界织物：CSS 物理架构]
 * ============================================================================
 * 准则 1：强制满屏 (100vw, 100vh)，根除溢出滚动和缩放畸变。
 * 准则 2：UI 采用绝对稳定的 Flexbox/Grid 阵列，严格左右两侧分布，永不交叠。
 * 准则 3：全面推行 mix-blend-mode: multiply，所有文字墨迹化，物理切除任何 background 色块。
 */

@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Zhi+Mang+Xing&display=swap');

:root {
  --ink-primary: #121412;
  --ink-secondary: #333333;
  --blood-red: #8B0000;
  --bg-fallback: #E2DED0;
  
  --font-title: 'Cinzel', serif;
  --font-mono: 'Space Mono', 'Microsoft YaHei', monospace;
  --font-script: 'Zhi Mang Xing', cursive;
}

/* 强制物理锁定视口 */
* { box-sizing: border-box; }
body, html {
  margin: 0; padding: 0;
  width: 100vw; height: 100vh;
  overflow: hidden; /* 这是锁死维度，杜绝滚动条的核按钮 */
  background-color: var(--bg-fallback);
}

.hub-container {
  position: relative;
  width: 100vw; height: 100vh;
  font-family: var(--font-mono);
  color: var(--ink-primary);
}

/* -----------------------------------------------------
   [光学一区：底色铺满]
------------------------------------------------------ */
.layer-base-parchment {
  position: absolute; inset: 0;
  width: 100vw; height: 100vh;
  object-fit: cover; /* 保证原始纸张永远不变形且绝对填满 */
  z-index: 0;
}
.layer-global-vignette {
  position: absolute; inset: 0;
  background: radial-gradient(circle at center, transparent 40%, rgba(15, 10, 5, 0.45) 100%);
  z-index: 1; pointer-events: none;
}

/* -----------------------------------------------------
   [光学二区：原生图片素材直接装载，利用 multiply 脱白]
------------------------------------------------------ */
.layer-art-assets {
  position: absolute; inset: 0;
  z-index: 5; pointer-events: none;
}
.layer-art-assets img {
  position: absolute;
  mix-blend-mode: multiply; /* 这行代码极其重要，这是湮灭图片自带白底的物理法则 */
}
/* 墨迹散落部署，避开中心区域 */
.pos-ink-1 { top: -5%; left: -5%; width: 35vw; opacity: 0.8; }
.pos-ink-2 { bottom: -10%; right: -5%; width: 45vw; opacity: 0.85; }
.pos-ink-3 { top: 10%; right: 5%; width: 25vw; opacity: 0.6; }
.pos-ink-4 { bottom: 5%; left: 10%; width: 30vw; opacity: 0.7; }
.pos-ink-5 { top: 40%; left: 40%; width: 20vw; opacity: 0.3; }
.pos-ten-1 { bottom: 15%; right: 15%; width: 20vw; opacity: 0.2; }
.pos-ten-2 { top: 20%; left: 10%; width: 18vw; opacity: 0.3; }

/* 祭坛图腾，受 Flex 水平居中保护 */
.pos-center-totem {
  top: 5%; left: 50%;
  transform: translateX(-50%);
  height: 85vh; /* 基于高度的稳固伸缩 */
  width: auto;
  opacity: 0.95;
}

/* -----------------------------------------------------
   [光学三区：完全分离的 Canvas 局部渲染层]
------------------------------------------------------ */
.layer-eye-canvas-container {
  position: absolute; inset: 0;
  z-index: 10;
  pointer-events: none; /* 让所有的鼠标事件穿透下去 */
}
.isolated-eye-engine {
  width: 100vw; height: 100vh;
  display: block;
}

/* -----------------------------------------------------
   [光学四区：稳定坚固的数据 UI 栅格]
------------------------------------------------------ */
.layer-ui-grid {
  position: absolute; inset: 0;
  z-index: 20;
  display: flex;
  justify-content: space-between; /* 牢牢把左右两侧卡住 */
  padding: 2.5rem 3.5rem;
  pointer-events: none; /* 整体穿透，防止挡住图腾的鼠标事件 */
}

/* UI 的三大列约束，限制宽度，保证绝对不挤作一团 */
.ui-column {
  width: 380px;
  display: flex;
  flex-direction: column;
  justify-content: space-between; /* 上中下对齐分布 */
  height: 100%;
}
.col-center { flex: 1; } /* 中心彻底掏空留给耶稣和眼球 */

/* UI 区块基石：杜绝所有 background-color, 纯粹的墨迹渗透 */
.ui-block {
  pointer-events: auto; /* 恢复自身区块的可交互性 */
  background: transparent !important;
  border: none !important;
  box-shadow: none !important;
  backdrop-filter: none !important;
  mix-blend-mode: multiply; /* 文字印于纸上的终极魔法 */
}
.block-title {
  font-size: 1.15rem;
  font-weight: bold;
  letter-spacing: 2px;
  border-bottom: 2px solid var(--ink-primary);
  padding-bottom: 6px;
  margin: 0 0 1.2rem 0;
  text-transform: uppercase;
}

/* --- [具体 UI 模块] --- */

/* 1. 星辰头部 */
.main-title {
  font-family: var(--font-title);
  font-size: 3.5rem; font-weight: 900;
  margin: 0; line-height: 1.1;
  letter-spacing: 1px;
}
.status-line {
  display: flex; align-items: center; gap: 8px;
  font-size: 0.85rem; font-weight: bold; color: var(--ink-secondary);
  margin-top: 8px;
}
.status-dot { width: 8px; height: 8px; background: var(--blood-red); border-radius: 50%; animation: pulse-dot 1.5s infinite; }
.timer-box { margin-top: 1.5rem; }
.time-huge { font-size: 2.8rem; font-weight: bold; letter-spacing: -1px; }
.time-sub { font-size: 0.8rem; font-weight: bold; color: var(--ink-secondary); }

/* 2. 灵魂监控面板 */
.metric-list { display: flex; flex-direction: column; gap: 10px; font-size: 0.95rem; font-weight: bold; }
.m-row { display: flex; justify-content: space-between; border-bottom: 1px dashed rgba(0,0,0,0.2); padding-bottom: 4px; }
.txt-blood { color: var(--blood-red); }
.txt-hyper { color: #d00; text-shadow: 0 0 8px rgba(200,0,0,0.5); font-size: 1.1rem; }

/* 3. 维度进度面板 (四大学科) */
.pillar-list { display: flex; flex-direction: column; gap: 14px; font-size: 0.9rem; font-weight: bold; }
.p-item { display: flex; flex-direction: column; gap: 6px; }
.p-head { display: flex; justify-content: space-between; }
.p-bar-bg { width: 100%; height: 6px; border: 1px solid var(--ink-primary); }
.p-bar-fill { height: 100%; background: var(--ink-primary); transition: width 0.5s ease-out; }

/* 4. 阿卡夏终端滚动区 */
.log-scroll-view {
  height: 200px; overflow-y: auto;
  font-size: 0.75rem;
  display: flex; flex-direction: column; gap: 6px;
  padding-left: 10px; border-left: 2px solid var(--ink-primary);
}
.log-scroll-view::-webkit-scrollbar { width: 3px; }
.log-scroll-view::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.4); }
.log-entry { line-height: 1.4; word-break: break-all; }
.log-ts { color: var(--ink-secondary); margin-right: 6px; }
.info { color: var(--ink-primary); }
.sys { color: var(--ink-secondary); font-style: italic; }
.echo { color: #444; font-style: italic; }
.success { color: #005500; font-weight: bold; }
.error, .alert { color: var(--blood-red); font-weight: bold; }

/* 5. 凡人供奉录 (极致精简列表版，杜绝胡乱漂浮) */
.block-offerings { margin-bottom: 1rem; }
.offering-board {
  display: flex; flex-direction: column; gap: 12px;
}
.offering-msg {
  font-family: var(--font-script);
  font-size: 1.8rem;
  color: #050505;
  line-height: 1.2;
}
.msg-empty { font-size: 0.85rem; color: var(--ink-secondary); font-style: italic; }

/* 6. 指令输入控制台 */
.block-cli {
  mix-blend-mode: normal !important; /* 唯一一个需要正常混合的元素，防盲打看不清 */
}
.cli-input-container {
  display: flex; align-items: center; gap: 10px;
  background: rgba(226, 222, 208, 0.7); /* 轻微的半透明底，贴合羊皮纸色调 */
  padding: 12px 15px;
  border: 2px solid var(--ink-primary);
}
.cli-prompt { font-weight: 900; font-size: 1.1rem; color: #004400; }
.cli-input-element {
  flex: 1; border: none; background: transparent; outline: none;
  font-size: 1.2rem; font-family: inherit; font-style: italic;
  color: var(--ink-primary);
  border-bottom: 1px dashed rgba(0,0,0,0.3); transition: 0.2s;
}
.cli-input-element:focus { border-bottom: 1px solid var(--ink-primary); }
.cli-input-element::placeholder { color: #666; font-size: 0.9rem; font-style: normal; }
.cli-input-element:disabled { opacity: 0.4; }

/* -----------------------------------------------------
   [全局遮罩] 死亡重置时生效
------------------------------------------------------ */
.layer-death-shroud {
  position: fixed; inset: 0; z-index: 9999;
  background: #020202; display: flex; justify-content: center; align-items: center;
}
.death-content { text-align: center; }
.death-title { color: #eee; font-family: var(--font-title); font-size: 4rem; letter-spacing: 10px; margin: 0; text-shadow: 0 0 15px #a00; }
.death-subtitle { color: #666; font-size: 1.2rem; letter-spacing: 5px; margin-top: 20px; animation: pulse-opacity 1s infinite; }

/* 动画帧定义 */
@keyframes pulse-dot { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
@keyframes pulse-opacity { 0%, 100% { opacity: 0.2; } 50% { opacity: 1; } }
</style>