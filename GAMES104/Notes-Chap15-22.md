# Notes-Chap15-22

## 15. Gameplay：基础

* Gameplay的特点
  * 会和其他各个系统进行交互，影响动画、渲染、音效、控制、etc
  * 迭代速度最快的一个系统（设计师给了新想法，就需要快速修改、实现）

### 15.1 Event Mechanism

* Event Mechanism
  * 观察者模式（Publish-subscribe Pattern）
  * 三步：Event Definition、Callback Registration、Event Dispatching
* **重点：Gameplay系统通常需要和引擎本身解耦，比如用纯数据的方式定义Event，否则游戏引擎可能会做成服务某种特殊游戏类型的引擎**
* Event Definition
  * 例子：Editor创建Event类型，然后Code Generator生成C++代码，再编译、反射进Editor
  * 如何避免加速、避免编译？
    * 可以使用DLL注入，避免重新编译代码
    * 也可以使用脚本语言，比如Lua、Python
* Callback Registration
  * 例子：受伤后，就会callback受伤函数
  * 问题：对象的生命周期 和 回调函数的安全性
    * 例子：手雷爆炸的时候，对象已经被销毁了，但是回调函数还是会访问这个对象进行处理
    * 解决方案：RAII
* Event Dispatch
  * 直接派发：问题很多，比如递归调用、难以并行等
  * Event Queue：用一个Queue捕获所有需要处理的Event
    * Event Queue需要使用一个Ring Buffer
    * 也可以设计多个不同种类的Queue，比如Net Event Queue、Combat Event Queue等

### 15.2 Game Logic

* 用C++等语言写逻辑的问题
  * 编译耗时、设计师和艺术家不一定熟悉编程语言
  * 解决方案：Scripting Languages，如Lua、Python
    * 优势：崩溃影响小等等
* **问题：GameObject的生命周期，是要引擎管理，还是脚本管理呢？**
  * 在玩法复杂的游戏中，通常是由脚本管理
  * 脚本语言会提供GC系统，优势是降低编码门槛，缺点是GC开销过大（在许多重度脚本语言的游戏中，GC的开销可能会占游戏逻辑开销的1/10以上）
* Scripting System
  * 引擎调用脚本：比如Unity，可以用C#扩展Component，然后Unity会调用对应代码
  * 脚本调用引擎：把引擎看作一个SDK集
* **问题：如何实现热更新？**
  * 通常可以通过直接修改函数指针来实现
  * 然后要避免热更新的脚本把  引擎炸掉
* **问题：脚本语言效率低**
  * 解决：**JIT，Just-in-time**
* 热门的脚本语言
  * Lua
  * Python
  * C#（可以编译成字节码）

### 15.3 Visual Scripting

* 基本元素：变量、语句和表达式、控制流、函数、（类）
* 会用不同颜色的引脚，来区别不同的数据类型
  * ![image-20240902112717536](./Notes-Chap15-22/image-20240902112717536.png)
* 函数 <-> 蓝图
  * ![image-20240902113925793](./Notes-Chap15-22/image-20240902113925793.png)
* 可视化编程的问题
  * 代码合并很困难，无法合并图
  * 蓝图可能会非常混乱
    * ![image-20240902114205293](./Notes-Chap15-22/image-20240902114205293.png)
* Visual Scripting 可以被编译成 Scripts，然后再由解释器或者VM执行

### 15.4 Character/Camera/Control (3C System)

* Character
  * 与其他系统的交互：Animation、Movement、Affects
  * 通常一个Character，会用一个状态机/ASM来表达所有动画状态，然后每个动画再辅助单独的脚本
* Control
  * 细节：辅助瞄准、震动反馈等等
* Camera
  * 细节
  * 可以调一调我的unity demo的camera系统

### 15.5 Takeaways

* 游戏逻辑太混乱，所以用Event系统简化逻辑
* 无法预先考虑所有逻辑，所以要开放scripting system
* scripting太难上手，所以提供了visual scripting

***

## 16. Gameplay：Basic AI

* Q&A环节：Event System通常会考虑events的优先级吗？
  * 可以分队列。但是在观察者模式中，不推荐这么做，因为这会让event的耦合性变高，并且不利于并行化。

* 【略！AI等以后有空再回来学吧！】

***

## 17. Gameplay: Advanced AI

* 【略！AI等以后有空再回来学吧！】

***

## 18. Online Gaming Architecture: Fundamentals

* Online Gaming的挑战
  1. 一致性：网络同步
  2. 可靠性：延迟、掉线重连
  3. 安全性：作弊、账号安全性
  4. 多样性：跨平台、热更新、不同类游戏
  5. 复杂性：高并发、高可用、高性能

### 18.1 Network Protocols

* OSI Model & TCP/IP Model
* Socket
* Protocols：TCP（连接稳定、拥塞控制等）、UDP（Connectionless、不可靠、高效）
  * ![image-20240903101528784](./Notes-Chap15-22/image-20240903101528784.png)

### 18.2 Reliable UDP

* 现代游戏引擎中，并不会直接用原生态的协议，而是会对协议做改造
  * 例子：Overwatch基于UDP，实现了一个自己的稳定的、带有延迟补偿的协议
* ACK、Negative ACK、SEQ、Timeouts
* Automatic Repeat Request（ARQ）
  * 使用一个滑动窗口来控制发包的数量
  * Stop-and-Wait ARQ：Window size = 1
  * Go-Back-N ARQ：窗口内丢包了，就重发整个窗口
  * Selective Repeat ARQ：窗口内丢包了，就重发特定包
* Forward Error Correction（FEC）
  * 目标：丢包可以直接不管，不需要重发
  * FEC方法
    * XOR-FEC
      * ![image-20240903102746429](./Notes-Chap15-22/image-20240903102746429.png)
      * 通过多的异或冗余packet，来复原数据
    * Reed-Solomon Codes
      * *听起来类似CRC，但好像细节不太一样*
* ![image-20240903103123793](./Notes-Chap15-22/image-20240903103123793.png)

### 18.3 Clock Synchronization

* Round-Trip Time（RTT）
  * 一个packet来回的耗时
  * RTT和Ping的不同：Ping是通过ICMP协议测量的，而RTT是应用层测量的
* Network Time Protocol（NTP）
  * 算法：估算出RTT，然后根据RTT来对齐时间
  * ![image-20240903104352155](./Notes-Chap15-22/image-20240903104352155.png)
  * 

