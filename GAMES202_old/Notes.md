# GAMES 202 Real-time High Quality Rendering

### Summary

##### Evolution of Real-Time Rendering

* Milestones
  * Programmable graphics hardware (shaders) (20 years ago)
  * Precomputation-based methods (15 years ago)
  * Interactive Ray Tracing (8-10 years ago)

***

### Recap of CG Basics

##### Basic GPU hardware pipeline

* Process
  * **Application**
  * ​	Input: vertices in 3D space
  * **Vertex Processing**
  * ​	Vertices positioned in screen space
  * **Triangle Processing**
  * ​	Triangles positioned in screen space
  * **Rasterization**
  * ​	Fragments (one per covered sample)
  * **Fragment Processing**
  * ​	Shaded fragments
  * **Framebuffer Operations**
  * ​	Output: image (pixels)
  * **Display**

##### OpenGL

* What is OpenGL
  * Is a set of APIs that cal the GPU pipeline from CPU
  * Cons
    * Fragmented: lots of different versions
    * C style, not easy to use
    * Cannot debug (?)
* How to use OpenGL
  * A. **Place objects/models**
    * Model specification
      * User specifies an object's vertices, normals, texture coords and send them to GPU as a **vertex buffer object** (VBO)
    * Model transformation
      * Use OpenGL functions to obtain matrices
  * B. **Set up an easel**
    * View transformation
      * Set camera by simply calling
    * Create / use a **framebuffer**
  * C. **Attach a canvas to the easel**
    * E. **you can also paint multiple pictures using the same easel**
      * 渲染一次，可以输出多种图像（phong、texture、normal...）
    * 输出方式
      * 直接输出到屏幕 -> 一帧还没渲染完，另一帧的画面覆盖进framebuffer -> 造成**画面撕裂**
      * 利用 垂直同步/双重缓冲/三重缓冲 避免画面撕裂
  * D. **Paint to the canvas**
    * This is when vertex / fragment shaders will be used
    * For each vertex in parallel
      * vertex shader (MVP, 插值)
    * For each primitive, OpenGL rasterizes
    * For each fragment in parallel
      * fragment shader (shading, lighting calculations, z-buffer depth test)
    * the most **importance**: **user-defined vertex, fragment shaders**
  * **Summary: in each pass**
    * Specify objects, camera, MVP, etc.
    * Specify framebuffer and input/output textures
    * Specify vertex / fragment shaders
    * (When you have everything specified on the GPU) Render!

##### OpenGL Shading Language

* Long histroy

  * In ancient times: assembly on GPUs
  * Stanfrod Real-Time Shading Language, work at SGI
  * Still long ago: Cg from NVIDIA
  * HLSL in DirectX (vertex + pixel)
  * GLSL in OpenGL (vertex + fragment)

* Shader Setup

  * Initializing

    * Create shader
    * Compile shader
    * Attach shader to program
    * Link program
    * Use program
  * Vertex Shader
  * Fragment Shader

* Debugging Shaders
  * Why shaders are **difficult to debug**?
    * You can't set a breakpoint or print something in GPU.
  * Years ago: NVIDIA Nsight with Visual Studio
    * Needed multiple GPUs for debugging GLSL
    * Had to run in software simulation mode in HLSL
  * Now
    * Nsight Graphics (cross platform, NVIDIA GPUs only)
    * RenderDoc (cross platform, no limitations on GPUs)
  * Yan's personal advice
    * Print it out!
    * ** Show values as colors!**
    * And use Color Meter.

##### The Rendering Equation

* 在Real-Time Rendering中，$L_i$指光源发出的光，而不是该点接收到的光。

  所以$L_i$需要乘上$Visibility$才等价于一般渲染方程。

***

### Real-Time Shadows

#### Shadow Mapping

* A 2-Pass Algorithm
  * Process
    * The light pass generates the SM
    * The camera pass uses the SM
  * An image-space algorithm
    * Pro: no knowledge of scene's geometry is required
    * Con: causing self occlusion and aliasing issues
  * Well known shadow rendering technique
  * Self Occlusion
    * ![3-1](pic\3-1.png)
    * 生成light pass图时，每个像素对应一个与像素平面平行的小平面，每个小平面可能会遮挡后一个平面。（如果不生成平面，只生成点的话，那么会产生更严重的条纹现象）
    * 光线以grazing angle射入时，self occlusion现象最严重
    * 荒野之息在夕阳时也会产生self occlusion
    * 处理方式：加入bias，不考虑过小的深度差
  * Detached Shadow
    * ![3-2](pic\3-2.png)
    * bias过大
  * Aliasing Issues
    * Cascaded Shadow Mapping 级联阴影（根据距离决定阴影分辨率）
* Second-depth shadow mapping
  * 每个光源对每个像素取第一小深度和第二小深度，在处理时取中间深度。（妙啊！）
  * 问题：需要每个物体均water-tight（封闭）
  * **RTR does not trust in COMPLEXITY**
    * RTR十分关注常数，on接受，o2n不接受

#### The math behind shadow mapping

* Inequalities in Calculus
  * ![3-3](pic\3-3.png)
  * 约等条件：
    * 积分域小；f(x)或g(x)的变化不大
  * ![3-4](pic\3-4.png)

#### Percentage Closer Soft Shadows (PCSS)

* Percentage Closer Filtering (PCF)
  * Provides **anti-aliasing** at shadows' edges
  * **Filtering the results of shadow comparisons**
  * 在shadow map上，找对应像素周围共N*N像素，都和shadingPoint判断遮挡后的结果

* Does filtering size matter?
  * Small -> sharper
  * Large -> softer

* Where is sharper? Where is softer?

  * 根据深度差判断？

* ![3-5](pic\3-5.png)

* **The complete algorithm of PCSS**
  * Step 1: Blocker search
    * getting the average blocker depth in **a certain region**
  * Step 2: Penumbra estimation
    * use the average blocker depth to determine filter size
  * Step 3: Percentage Closer Filtering

* Which region to perform blocker search?
  * Can be set constant (e.g. 5x5), but can be better with heuristics

  ![3-6](pic\3-6.png)

* ![3-7](pic\3-7.png)

  * Dying Light 实例，树根处阴影硬，树叶处阴影软

* 问题：难以处理多光源，n个光源需要n倍处理时间

 *闫老师被领导收走的的PS4*

*领导的决定也是你能质疑的？*

#### More on PCF and PCSS

* Filter / Convolution ≈ 加权平均数
  * $[w*f](p) = \sum\limits_{q\in 邻域(p)}w(p,q)f(q)$
* In PCSS
  * $V(x) = \sum\limits_{q\in 邻域(p)}w(p,q)\cdot\chi^+[D_{SM}(q)-D_{scene}(x)]$
    * $\chi$ 函数，变量值>0则为1，变量值<0则为0
* 重点：PCF 不是在 Shadow Map 或者 图像 上做模糊！
* 考虑 PCSS 的三步算法，哪步耗时最大？
  * 第一步和第三步：在一个区域内检测所有texel
    * 检测区域内所有texel => 随机取样texel(会导致噪声/Flicker)
    * Flicker指：每帧噪声不同，相邻帧连续播放，导致画面抖动
  * 方法：随机取样texel，来降低时间开销，再对图像空间进行降噪

#### Variance Soft Shadow Mapping

##### Step3的优化

* 在第三步中，在比较 Shadow Map 和 Shading Point 的深度时，可以采用近似（正态分布）

  * 只需要知道深度的期望和方差
* Key Idea

  * Quickly compute the **mean** and **variance** of depths in an area

    * 快速得到一个区域内的深度均值和方差
  * **Mean** (Expectation)
    * 采用 MIPMAP !
    * 采用 Summed Area Tables (SAT)
  * **Variance**
    * $Var(X) = E(X^2)-E^2(X)$
      * 为了计算 $E(X^2)$ ，需要额外的一张记录**深度平方**的Shadow Map (和普通的Shadow Map一起生成)
      * 还需要深度平方的均值
  * 利用 CDF 计算 有多少点比ShadingPoint近
    * ![4-1](pic\4-1.png)
  * VSSM又找到了 Chebychev不等式
    * $P(x>t)\le \Large\frac{\sigma^2}{\sigma^2+(t-\mu)^2}$
    * ![4-2](pic\4-2.png)
    * 发现了Chebychev不等式，正态分布就不用考虑了
* Performance
  * Shadow map generation:
    * "square depth map": parallel, along with shadow map
  * Run time
    * Mean of depth in a range: O(1)
    * Mean of depth square in a range: O(1)
    * Chebychev: O(1)
    * **No samples / loops needed!**
  * 缺点：
    * 物体或者光源移动，需要重新生成Mipmap（但时间可以忽略）

##### Step1的优化

* Back to Step 1: blocker search
  * 要求的是遮挡物的平均深度，而不是所有texel的平均深度
* Key Idea:
  * Blocker (z < t), avg. $z_{occ}$ (we want to compute)
  * Non-blocker (z > t), avg. $z_{unocc}$
  * $\Large\frac{N_1}{N}\normalsize z_{unocc} + \Large\frac{N_2}{N}\normalsize z_{occ} = z_{avg}$
    * $N$ 为物体数量，$N1$ 为非遮挡物数量，$N2$ 为遮挡物数量
  * Approximation: N1/N = P(x > t)，Chebychev!
  * Approximation: N2/N = 1 - P(x > t)
  * $z_{unocc}$, we really don't know
  * Approximation: $z_{unocc}=t$
    * 更大胆的假设，假设所有非遮挡点的深度和ShadingPoint相同

##### 现在

因为人们对图像空间降噪技术的提升，大家对噪声的容忍度提升，所以现在PCSS用的更多。

#### MIPMAP and Summed-Area Variance Shadow Maps

* Mipmap的范围查询不够准，引入Summed-Area Table
* 注：范围求和 = 范围求平均

##### Summed-Area Table

* 就是二维数组前缀和，O(n)构建，O(1)算区间和
* SAT是数据结构，对应的算法是前缀和

#### Moment Shadow Mapping

* 若遮挡物不能近似为正态分布，则阴影会出现变暗或者变亮的情况
  * 变暗可能看不出来，但变亮则会很突兀
* Moment Shadow Mapping 解决 VSSM 中描述分布不准的问题
* Key Idea
  * 使用 更高阶的矩(Moment) 来描述分布
  * 从这个角度看，VSSM只使用了一阶和二阶的矩，我们可以用更高阶的矩来描述遮挡物
  * 类似与一些展开(如泰勒展开)
* 正常情况下，用4阶矩够用了
* 用 m阶矩，在CDF中会产生 m/2 个台阶(转折)
* 难点：利用 前m阶矩 来计算出一个 CDF函数
  * 具体参考Peter的paper
* 缺点：会消耗更多的空间；计算函数耗时
* 效果：![4-3](pic\4-3.png)
