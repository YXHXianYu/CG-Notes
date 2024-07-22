Computer Graphics Summary

> YXH咸鱼的总结笔记

***

## Linear Algebra 线性代数

#### Vector 向量

* Normalization
* Addition
  * Geometrically : Parallelogram law & Triangle law
  * Algebraically : Simply add coordinates
* Cartesian Coordinate
* Multiplication
  * **Dot Product**
    * $
      \vec{a}\cdot\vec{b}=||\vec{a}||\ ||\vec{b}||\ cos(\theta)\\
      cos(\theta)=\tfrac{\vec{a}\cdot\vec{b}}{||\vec{a}||\ ||\vec{b}||}
      $

  * **Cross Product**
    * $
      长度定义：||\vec{a}\times\vec{b}||=||\vec{a}||\                 ||\vec{b}||\sin(\phi)\\
      方向定义：右手螺旋定则：四指由\vec{a}旋转向\vec{b}，大拇指方向即结果向量方向\\
      交换律: \vec{a}\times\vec{b}=-\vec{b}\times\vec{a}\\
      \vec{a}\times\vec{a}=0
      $

#### Matrices 矩阵

* Matrix-Matrix Multiplication

  * $$
    (M\times N) (N \times P) = (M \times P)\\c[i,j] = \sum_{1\le k\le N}(a[i,k]\times b[k,j])
    $$

* Matrix-Vector Multiplication

* Transpose of a Matrix

  * Switch rows and columns

* Identity Matrix and Inverses

  * $$
    xI_{3\times 3} = \left (\begin{array}{ccc}1 & 0 & 0\\0 & 1 & 0\\0 & 0 & 1\\\end{array}\right)\\AA^{-1} = A^{-1}A = I\\(AB)^{-1}=B^{-1}A^{-1}
    $$

* Vector multiplication in Matrix from

  * Dot product
  * Cross product

***

## Transformation 变换

#### 2D Transformations 二维变换

* Scale Matrix

  * $
    \begin{bmatrix}
    x^{'}\\y^{'}
    \end{bmatrix}
    =
    \begin{bmatrix}
    s_x & 0\\0 & s_y
    \end{bmatrix}
    \begin{bmatrix}
    x\\y
    \end{bmatrix}\begin{bmatrix}
    x^{'}\\y^{'}
    \end{bmatrix}
    =
    \begin{bmatrix}
    s_x & 0\\0 & s_y
    \end{bmatrix}
    \begin{bmatrix}
    x\\y
    \end{bmatrix}
    $

* Reflection Matrix

  * $
    \begin{bmatrix}x^{'}\\y^{'}\end{bmatrix}=\begin{bmatrix}-1 & 0\\0 & 1\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}
    $

* Shear Matrix

  * $
    \begin{bmatrix}x^{'}\\y^{'}\end{bmatrix}=\begin{bmatrix}1 & a\\0 & 1\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}
    $

* Rotate Matrix

  * 我直接自己推出来了，耶！不愧是我 (✿◡‿◡)

    - "由特殊到一般"

  * $\mathbf{R_\theta} =
    \begin{bmatrix}
    \cos\theta&-\sin\theta\\
    \sin\theta&\cos\theta
    \end{bmatrix}
    $

  * $\mathbf{R_{-\theta}}=\mathbf{R^{T}_{\theta}}$

  * $\mathbf{R_{-\theta}}=\mathbf{R^{-1}_{\theta}}\ (by\ \ definition)$ 

#### Homogeneous Coordinates 齐次坐标系

* 通过增加一个矩阵维度w，把所有线性变换统一为一个矩阵
  * 2D Point = $(x,y,1)^T$
  * 2D Vector = $(x,y,0)^T$
* 增加w维度后，平移变换可以和其他旋转等变换统一
  * 因为向量具有平移不变性，所以对向量做平移变换，不改变向量自身。
    * 向量受平移变换影响，所以w为0，不受影响。
    * 点受平移变换影响，所以w不为0，受影响。
* 2D 齐次坐标系中，$\left( \begin{array}{c} x\\y\\w \end{array} \right) =\left( \begin{array}{c} x/w\\y/w\\1 \end{array} \right), w \ne 0$
* Inverse Transform   <=>   Inverse Matrix
  * 逆变换 同 逆矩阵
* Composite Transform
  * 可以把许多变换压成一个单一矩阵

#### 3D Transformation 三维变换

* Rotation around x-, y-, z-axis

* y轴旋转因为坐标系的定义(左右手)和叉乘，所以形式和x/z轴不同

* **Rodrigues' Rotation Formula 罗德里格旋转公式**

  * $R(n, \alpha) 表示 n轴旋转\alpha°$

$$
\textbf{K} = 
\begin{bmatrix}
0 & -n_z & n_y\\
n_z & 0 & -n_x\\
-n_y & n_x & 0
\end{bmatrix}
\\
\textbf{R}(\vec{n},\alpha)
= I
+ sin(\alpha)K
+ (1-cos(\alpha))K^2
$$

* GAMES101默认中为 左手系

***

## Viewing Transformation 视角变换

* **View / Camera Transformation**

* MVP(model view projection) Transformation

* Define the camera
  * Position $\vec{e}$
  * Look-at / gaze direction $\hat{g}$
  * Up direction $\hat{t}$

* Transform the camera to

  - The origin, up at Y, look at -Z

* **Perspective Transformation**
  $$
  \begin{bmatrix}
  n & 0 & 0 & 0\\
  0 & n & 0 & 0\\
  0 & 0 & n+f & -nf\\
  0 & 0 & 1 & 0
  \end{bmatrix}
  $$


***

## Rasterization 光栅化



#### Anti-aliases

* 傅里叶级数展开
  - Represent a function  as a weighted sum of sines and cosines.
  - 任何一个周期函数都可以用一系列正余弦函数的线性组合表示
* 傅里叶变换
  - 多项式 转换为 极坐标下的点集？
  - 时域 变为 频域
* Aliases（走样）
  - 同一个采样方法采样两种不同频率的函数，得出来的结果无法区分
* Filtering
  - 高频滤波（此处，图片转频域，高通滤波器，转回图片）
  - ![6-1](I:\计算机图形学\GAMES101闫\pic\6-1.png)
  - 高频：边界处，变化大有断层，就可以看成高频
  - 同理，还有低通滤波器，或者低频高频一起滤
  - Filtering = Convolution (= Averaging)
* 卷积定理： 
  - 时域上卷积 等价于 频域上乘积
  - 时域上乘积 等价于 频域上卷积
* Reduce Aliasing Error
  - Option 1: increasing sampling rate
  - Option 2: Antialiasing
    - 先模糊 (低通滤波)，再采样
      - 模糊：用小盒子滤波器对图像取平均（要处理边界像素的亮度）
      - 采样：判断像素是否在三角形内
* MSAA：Multisampling / Supersampling
  - 作用：模糊图像，并对图像进行采样
  - 内容：把1个像素划分成 $n$ 小像素，再求出 $m$ 个像素点在三角形内，则像素覆盖率为 $\frac{n}{m} \times 100\%$ 
  - 缺点：增大计算量，速度慢
  - 重点：SSAA和MSAA的不同是什么？
* FXAA：Fast Approximate AA
  - 找到图像边界，用无锯齿图像替换有锯齿边界
* TAA：Temporal AA (po重音)
  - 找上一帧来进行抗锯齿
* Super resolution / super sampling (超分辨率/超级采样)
  - DLSS（Deep Learning Super Sampling）

***

## Shading 着色

#### Visibility 可见性

* Painter's Algorithm 画家算法
  * 时间复杂度 O(nlogn + n)
  * 无法处理 “A叠B、B叠C、C叠A” 的情况
  * ![7-1](pic\7-1.png)
* Z-Buffer 深度缓存
  * 时间复杂度 O(n)
  * ![8-1](pic\7-2.png)

#### Blinn-Phong Reflection Model 布林冯反射模型

* 光照 = 环境光Ambient + 漫反射Diffuse + 高光Specular
  * ![8-3](pic\8-3.png)
* Diffuse
  * $L_d$ = $k_d$ $\cdot$ $(I/r^2)$ $\cdot$ $max(0, \vec{n}\cdot \vec{l})$
* Specular
  * $L_s = k_s\cdot(I/r^2)\cdot max(0,\vec{n}\cdot\vec{h})$
    * 这里点乘的$cos(\theta)$的次数需要为100+，来实现高光只有一小部分的特点
* Ambient
  * $L_a = k_aI_a$
* 和**Phong Reflection Model**的区别在于光线与观测角度差的计算方法

#### Shading Frequencies 着色频率

* **Flat** Shading
  - 计算 **三角形(面)** 法线，三角形光照直接计算得到
* **Gouraud** shading
  - 计算 **点** 的法线，三角形光照对顶点光照取均值
  - 顶点法线 取 关联的面的法线 的 加权平均值
* **Phong** shading
  - 计算 **像素** 的法线
  - 所有法线都是单位向量，需要 **归一化**
* ![8-4](pic\8-4.png)

#### Graphics Pipeling (Real-time Rendering) 图形管线

* Pipeline 管线
  * 概念：三维模型经过一系列操作渲染到二维平面上的 **一系列操作**
  * 流程
    1. Vertex Processing
    2. Triangle Processing 
    3. Rasterization 光栅化
    4. Fragment Processing 
    5. Framebuffer Operations 
* Shader 着色器
  * 推荐网站 : [Shadertoy](http://shadertoy.com/view/ld3Gz2)

#### Texture mapping 纹理映射

#### Barycentric coordinates 重心坐标

* 注意点：重心坐标没有投影不变性
  * 所以三维空间的属性需要在三维空间中做插值。

#### Texture antialiasing (Mipmap)

* 纹理过小 与 纹理过大

#### Applications of Texture

* Environment Map 环境光照
  * 注：认为所有光照来自无限远
  * Spherical Map ：描述来自不同方向的环境光信息(在一个球的表面)
  * Cube Map ：描述来自不同方向的环境光信息(在一个立方体的表面(沿球心连线延长))
* Bump Mapping (凹凸贴图)

***

## Geometry 几何

#### Explicit 显式
1. Point Cloud 点云 (explicit) 

   1. 可以用特别密集的点云来表示模型
   2. 常用于扫描得到的数据

2. Polygon Mesh(explicit)

   1. 最广泛应用的显式表示

3. The Wavefront Object File (.obj)  Format

   1. 储存一个模型的文件

   2. 例如，储存一个立方体

      1. 顶点坐标
      2. 纹理坐标
      3. 法线
      4. 哪3个点组成1个三角形（参数：第i个顶点，第j个纹理坐标，第k个法线）  

#### Curves 曲线
1. Camera Path
2. Animation Curves
3. Vector Fonts 矢量字体

#### Bezier Curves 贝塞尔曲线（implicit）
* 用一系列控制点来定义曲线
* Evaluating Bezier Curves
  * de Casteljau Algorithm
    * 递归找bn和bn+1之间位于t的点
    * 插值
    * 代数：给n+1个控制点，可以得到一个n阶的贝塞尔曲线....
  * Bernstein Polynomials
* Properties
  * 过起末点
  * 切线
  * 反射后的控制点得出的贝塞尔曲线不变
  * 贝塞尔曲线在控制点形成的凸包(Convex Hull)内
* Beziewise Bezier Curves
  * 每n个控制点定义一个贝塞尔曲线（大家喜欢n=4）
  * 连接点和前后两个控制点，三点共线（最好连接点到两个控制点距离相等，大家喜欢的额外定义）（三点共线：C0连续(continuity) ）（三点共线且距离相等：C1连续）（cn即n阶导）
* Splines 样条
  * 一个可控的曲线
* B-splines B样条
  * 极其复杂
  * NURBS 非均匀游离B样条

***

## Ray Tracing 光线追踪

* Why Ray Tracing?
  * Rasterization couldn't handle **global** effects well
    * Soft shadows
    * Glossy reflection
    * Indirect illumination
  * Rasterization is fast, but quality is relatively low

* Light Rays
  * Three ideas about light rays

     1. Light travels in straight lines

     1. Light rays do not "collide" with each other if they cross

     3. Light rays travel from the light sources to the eye

        "And if you gaze long into an abyss, the abyss also gazes into you." -- Nietzsche (translated)

* Ray Casting
  * Pinhole Camera Model
    * 眼睛对每个像素发出一条光线，交于最近的物体的一点
    * 该点对光源连线，并着色

![11-1](pic\11-1.png)

### Whitted-Style Ray Tracing

#### Recursive Ray Tracing **递归**的光线追踪

* Whitted-Style光线追踪就是在模拟光线不断投射的过程

* 如何计算光线与球面的交点？

  - 定义光线 : $\textbf{r}(t) = \textbf{o}+t\textbf{d},\ 0 \leq t \leq \infty $
    - 光线被 它的起点o 和 一个向量d 定义
  - 定义平面: $\textbf{p}:(\textbf{p}-\textbf{c})^2-R^2=0$

  * 交点 : $(\textbf{o}+t\textbf{d}-\textbf{c})^2-R^2=0$
  * 作为二次函数，即 $ at^2 + bt + c = 0 $
    * $ a = \textbf{d} \cdot \textbf{d} $
    * $ b = 2(\textbf{o} - \textbf{c}) \cdot \textbf{d} $
    * $ c = (\textbf{o} - \textbf{c}) \cdot (\textbf{o} - \textbf{c}) - R^2 $
    * $ t = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a} $

* 如何计算光线与隐式表面的交点？

  * General implicit surface : $ \textbf{p} : f(\textbf{p}) = 0 $
  * Substitute ray equation : $ f(\textbf{o}+t\textbf{d}) = 0 $
  * Solve for **real, positive** roots

* 如何计算光线与显式表面的交点？

  * 问题：根据交点数量的奇偶性，来判断点是否在模型内：如果光线切于模型怎么办？
  * 最暴力的方法：枚举每个三角形，并且判断是否相交

* 如何得出光线和三角形的交点？

  * 分解问题：1.光线和平面求交；2.交点是否在三角形内
    * 定义平面：一个法线$\textbf{N}$ + 一个点$\textbf{p'}$
      * 平面上任意一个点P满足：$ (\textbf{p} - \textbf{p'}) \cdot \textbf{N} = 0 $
      * 得到一个平面方程 $ ax+by+cz+d = 0 $
    * 交点：$ (\textbf{o}+t\textbf{d} - \textbf{p'}) \cdot \textbf{N} = 0 $
      * 一次方程，解出 t，检查 t 存在
  * **Moller Trumbore Algorithm**
    * 联立 光线方程 和 三角形重心坐标方程
      * 未知数：时间t，重心坐标b1,b2（b3 = 1-b1-b2)
      * 空间中三维坐标，三个方程组
      * 三个方程三个未知数，可解
      * t 为正数，b1、b2、b3均非负数，则光线与三角形存在交点
      * 可以利用高斯消元求解（复杂度高）
      * 可以利用本算法(基于Cramer法则/矩阵行列式=三个列向量的混合积)求解
        * 推导：https://blog.csdn.net/zhanxi1992/article/details/109903792

* 如何加速得出光线和表面的交点？

  * Bounding Volumes/Box 包围体积/盒
    * 如果光线连包围盒都碰不到，那里面的东西根本不用看
  * **Axis-Aligned Bounding Box (AABB) 轴对齐包围盒**
  * 如何得出光线和轴对称包围盒的交点
    * 对每个对面计算出$t_{min}$和$t_{max}$（利用光线和平面相交的判断方法）
    * 光线进入盒子的时间$t_{enter}=max(t_{min})$，光线离开盒子的时间$t_{exit}=min(t_{max})$
    * 如果$t_{enter}<t_{exit}$，则光线存在一段时间进入盒子；
    * 如果$t_{exit} < 0$，则盒子在光线背后；
    * 如果$t_{exit}>=0 \ \ and\ \  t_{enter}<0$，则起点在盒子里，光线在盒子里，存在交点；
    * **总结：$ t_{exit}>=0 \ \ \&\&\ \   t_{enter}<t_{exit}$，则光线与盒子相交**
  * 轴对齐包围盒的优势，是可以用每个轴的分量来计算相交时间，效率高

![11-2](pic\11-2.png)

#### Spatial Partitions 空间划分

* Uniform Grids
* Spatial Partitioning Examples 空间划分实例
  * Oct-Tree : 八叉树（划分立方体为八个子立方体）
  * **KD-Tree**：二叉树（每次划分x -> y -> z轴）
  * BSP-Tree ：划分面非轴对齐；高维复杂；
* Data Structure of KD-Tree
  * Internal nodes store
    * split axis: x-, y-, or z-axis
    * split position: coordinate of split plane along axis
    * children: pointers to child nodes
    * **No objects are stored in internal nodes**
  * Leaf nodes store
    * list of objects
  * 非叶子结点存KDtree信息，叶子结点存objects列表
  * 缺点：
    * 判定三角形和AABB是否有交点 --- 这个问题很难解决；
    * 一个物体有可能出现在多个叶子节点中；

#### Object Parititions 模型划分

* Bounding Volume Hierarchy (BVH)

  * 把一个包围盒中的物体分成两堆，重新求出包围盒，作为新节点，递归求解。若一个节点中物体足够少，则作为叶子节点。
  * 优点：

    * 性质：一个物体只会出现在一个包围盒/节点中
    * BVH比KD-Tree好写，不用涉及复杂三维运算
  * 缺点：可能出现相似的包围盒，但是内含三角形不同（不如合并在一起）
  * Building BVHs
    * How to subdivide a node?
      * Choose a dimension to split
      * Heuristic #1: Always choose the longest axis in node
      * Heuristic #2: Split node at location of **median** object（取位置位于中间的物体划分（取三角型重心的x坐标中位数的三角形））
        * 无序数列取第i大的数 - 快速选择算法：时间复杂度O(n)
    * Termination criteria?
      * Heuristic: stop when node contains few elements (e.g. 5)
  * BVH不支持动态场景

* Data Structure for BVHs
  * Internal nodes store
    * Bounding box
    * Children: pointers to child nodes
  * Leaf nodes store
    * Bounding box
    * List of objects
  * Nodes represent subset of primitives in scene

    * All objects in subtree

* BVH Travelsal (BVH遍历)

  * 类似线段树，很基础

* **Surface Area Heuristic**

  * According to [BVH with SAH](https://www.cnblogs.com/lookof/p/3546320.html)

  * BVH加速结构需要考虑两个阶段的工作：Build 和 Traversal

    1.Build 构建

    ​	1.1 Principle 原则：每个object的质心包围盒的最长轴 为 划分轴

    ​	1.2 Strategy 策略：

    ​		1.2.1 SPLIT_MIDDLE 中点划分

    ​		1.2.2 SPLIT_EQUAL_COUNT 等量划分

    ​		1.2.3 SPLIT_SAH 表面积启发式算法划分：

    ​		  ->  把 **表面积比率** 看作是 **被光线射中的概率** ；

    ​		  ->  使得 **S(A)/S(C)\*N(A) + S(B)/S(C)\*N(B)** 最小

    ​	1.3 Compact BVH：就是把指针关系改为数组关系，可以优化结构加速遍历

    2.Traversal 遍历

### Basic Radiometry 辐射度量学

* 精确定义光的各种属性和与其他物体的交互，才能模拟出漂亮真实的场景。

* Radiometry：
  * Measurement system and units for illumination
  * Perform lighting calculations **in a physically correct manner**

* 基于几何光照

  * New terms: Radiant flux, intensity, irradiance, radiance

* **Radiant Energy**
  * Radiant energy is the energy of electromagnetic radiation.
  * $Q \ [J = Joule]$  

* **Radiant Flux** (Power) 
  * Radiant flux is the energy emitted, reflected, transmitted or received, per unit time.
  * $Φ = \Large\frac{dQ}{dt}\normalsize \ [W = Watt]\ [lm = lumen]$

* **Radiant Intensity**

  * The radiant (luminous) intensity is the power per unit **solid angle** emitted by a point light source
  * $ I(\omega) = \Large\frac{d\Phi}{d\omega}\ \normalsize[\Large\frac{w}{sr}\normalsize] \  [\Large\frac{lm}{sr} \normalsize =  cd = candela] $

* **Angles and Solid Angles**

  * Angle: ratio of subtended arc length on circle to radius
    * $ \theta = \Large \frac l r $
    * Circle has $ 2\pi $ **radians**
  * **Solid angle**: ratio of subtended area on sphere to radius squared
    * $ \Omega = \Large \frac A {r^2} $
    * Sphere has $ 4\pi ​$ **steradians**
  * $ dA = (r \ d\theta)(r\ sin\theta\ d\phi) =r^2\ sin\theta\ d\theta\ d\phi$
    * $ \theta,\ \phi $ 分别是表示球上单位向量的角度
  * 微分立体角 $ d\omega = \Large\frac{dA}{r^2}\normalsize=sin\theta\ d\theta\ d\phi $
  * 如果一个点光源均匀向外辐射光线，根据积分，每个方向光线的intensity: $ I = \Large\frac{\Phi}{4\pi} $

* **Irradiance**
  * The irradiance is the power per (perpendicular/projected) unit area incident on a surface point.
  * $ E(x) = \Large\frac{d\Phi(x)}{dA} \normalsize\ [\Large\frac{W}{m^2}\normalsize]\ [\Large\frac{lm}{m^2}\normalsize=lux] $
  * 只考虑垂直于平面的入射光的能量
    * **Lambert's Cosine Law** 若入射光线不垂直于平面，需要投影到垂直方向
  * Correction: **Irradiance** Falloff
    * 一根极细光锥传播时，对应的Solid Angle不变，则Intensity不变；随着传播距离(半径)增大，Irradiance逐渐衰减。
    * 故光线传播不改变Intensity，改变Irradiance

* **Radiance**

  * The radiance is the power emitted, reflected, transmitted or received by a surface, per unit solid angle, per projected unit area.
  * $ L(p, \omega) = \Large \frac{d^2\Phi(p,\omega)}{d\omega\ dA\ cos\theta} \normalsize $
  * Incident Radiance
    * Irradiance per solid angle
    * $ L(p, \omega) = \Large \frac{dE(p)}{d\omega\  cos\theta} \normalsize $
  * Exiting Radiance
    * Intensity per projected unit area
    * $ L(p, \omega) = \Large \frac{dI(p,\omega)}{dA\ cos\theta} \normalsize $
  * **Irradiance & Radiance**
    * $dE(p,\omega)=L_i(p,\omega)\ cos\theta\ d\omega$
    * ​       $E(p)=\Large\int_{\normalsize H^2}\normalsize L_i(p,\omega)\ cos\theta\ d\omega$
    * Unit hemisphere: $H^2$

* **Bidirectional Reflectance Distribution Function (BRDF)**
  $$
  f_r(\omega_i \rightarrow \omega_r)
  = \frac{dL_r(\omega_r)}{dE_i(\omega_i)}
  = \frac{dL_r(\omega_r)}{L_i(\omega_i)\ cos\theta_i\ d\omega_i}
  \ [\frac{1}{sr}]
  $$

  * **The Reflection Equation 反射方程**
      * $L_r(p,\omega_r)=\Large\int_{\normalsize H^2}\normalsize f_r(p,\omega_i\rightarrow\omega_r)\ L_i(p,\omega_i)\ cos\theta_i\ d\omega_i$
  * Challenge: Recursive Equation
      * 出射光线由入射光线决定，反射成为一个递归问题
  * **The Rendering Equation 渲染方程**
      * $L_o(p,\omega_o)=L_e(p,\omega_o)+\Large\int_{\normalsize\Omega+}\normalsize L_i(p,\omega_i)f_r(p,\omega_i,\omega_o)(n\cdot\omega_i)\ d\omega_i$
          * $cos\theta_i = n\cdot\omega_i$
      * $L(u) = e(u) + \int\ I(v)\ K(u,v)\ dv$
      * 看作：$L = E + KL$
          * 转换为：$L = (I - K)^{-1}E$
          * 展开为：$L = E + KE + K^2E + K^3E + ...$
      * **Shading in Rasterization**：$L = E + KE$
  * **Global Illumination**
      * 直接光照(直接光源+单次反射) + 间接光照(多次反射)

### Probability Review 概率论复习

* Probability Distribution Function (PDF) 概率密度函数
  * $p(x) >= 0 and \Large\int\normalsize p(x) \ dx=1$
  * $E[X] = \Large\int\normalsize x\ p(x)\ dx$

### Monte Carlo Integration 蒙特卡洛积分

* What & How: Estimate the integral of a function by averaging random samples of the function's value.
* Monte Carlo Estimator
  * Definite integral: $\Large\int^b_a\normalsize f(x)dx$
  * Random variable: $X_i\sim p(x)$
  * **Monte Carlo Estimator**: $F_N=\Large\frac{1}{N}\sum\limits^{N}_{i=1}\frac{f(X_i)}{p(X_i)}\normalsize$
* **Uniform** Monte Carlo Estimator
  * Definite integral: $\Large\int^b_a\normalsize f(x)dx$
  * **Uniform** Random variable: $X_i\sim p(x) = \Large\frac{1}{b-a}$
    * 随机变量均匀采样 且 均等
  * **Basic** Monte Carlo Estimator: $F_N=\Large\frac{b-a}{N}\sum\limits^{N}_{i=1}\normalsize f(X_i)$
* $F_N=\Large\int\normalsize f(x)dx=\Large\frac{1}{N}\sum\limits^{N}_{i=1}\frac{f(X_i)}{p(X_i)}\normalsize$
* Some notes:
  * The more samples, the less variance.
  * Sample on x integrate on x. 在积分域上采样.

### Path Tracing! 路径追踪！

* the Problems of Whitted-Style Ray Tracing
  1. Glossy材质的物体不一定全为全反射 [The Utah Teapot]
  2. 光线打到Diffuse材质的物体时，应该继续反射，产生**ColorBleeding** [The Cornell box]
* the  Rendering Equation involoves
  1. Solving an integral over the hemisphere
  2. Recursive execution
* A Simple Monte Carlo Solution
  * The Rendering Equation : $L_o(p,\omega_o)=\Large\int_{\normalsize\Omega+}\normalsize L_i(p,\omega_i)f_r(p,\omega_i,\omega_o)(n\cdot\omega_i)\ d\omega_i$

    * 省略了发光项
  * Monte Carlo integration : $\Large\int\normalsize f(x)dx=\Large\frac{1}{N}\sum\limits^{N}_{i=1}\frac{f(X_i)}{p(X_i)}\normalsize$
  * What's our "f(x)" : $L_i(p,\omega_i)f_r(p,\omega_i,\omega_o)(n\cdot\omega_i)\ d\omega_i$
  * What's our pdf : $p(\omega_i)=1/2\pi$

    * 半球面
  * **In General**
    * $L_o(p,\omega_o)=\Large\int_{\normalsize\Omega+}\normalsize L_i(p,\omega_i)f_r(p,\omega_i,\omega_o)(n\cdot\omega_i)\ d\omega_i$

    * ​                   $\approx \Large\frac{1}{N}\sum\limits_{i=1}^{N}\frac{L_i(p,\omega_i)f_r(p,\omega_i,\omega_o)(n\cdot\omega_i)}{p(\omega_i)}$

    * ````
      shade(p, wo)
      	Randomly choose N directions wi~pdf
      	Lo = 0.0
      	For each wi
      		Trace a ray r(p, wi)
      		If way r hit the light
      			Lo += (1 / N) * L_i * f_r * cosine / pdf(wi)
      		Else If ray r hit an object at q
      			Lo += (1 / N) * shade(q, -wi) * f_r * cosine / pdf(wi)
      	Return Lo
      ````

#### Path Tracing

* Shader

  ````
  shade(p, wo)
  	Randomly choose ONE directions wi~pdf(w)
  	Trace a ray r(p, wi)
  	If way r hit the light
  		Return L_i * f_r * cosine / pdf(wi)
  	Else If ray r hit an object at q
  		Return shade(q, -wi) * f_r * cosine / pdf(wi)
  ````
   * **路径追踪 N = 1**
   * 分布式光线追踪 N > 1

 * Ray Generation
   ````
     ray_generation(camPos, pixel)
     	Uniformly choose N sample positions within the pixel
     	pixel_radiance = 0.0
     	For each sample in the pixel
     		Shoot a ray r(camPos, cam_to_sample)
     		if ray r hit the scene at p
     			pixel_radiance += 1 / N * shade(p, sample_to_cam)
     	Return pixel_radiance
   ````

* Russain Roulette (RR) 俄罗斯轮盘赌
  * With probability P, shoot a ray and return the **shading result divided by P: Lo/P**
  * With probability 1-P, dont shoot a ray and you'll get **0**
  * In this way, you can still **expect** to get Lo : $E = P * (Lo / P) + (1 - P) * 0 = Lo$
  ````
  shade(p, wo)
  	Manually specify a probability P_RR
  	Randomly select ksi in a uniform dist. in [0, 1]
  	If ksi > P_RR
  		Return 0.0;
  
  	Randomly choose ONE directions wi~pdf(w)
  	Trace a ray r(p, wi)
  	If way r hit the light
  		Return L_i * f_r * cosine / pdf(wi) / P_RR
  	Else If ray r hit an object at q
  		Return shade(q, -wi) * f_r * cosine / pdf(wi) / P_RR
  ````

* Samples per pixel (SPP) 采样率

* Sampling the Light

  * $L_o(p,\omega_o)=\Large\int_{\normalsize\Omega+}\normalsize L_i(p,\omega_i)f_r(p,\omega_i,\omega_o)\ cos\theta\ d\omega_i$
  * ​                       $\Large\int_{\normalsize A}\normalsize L_i(p,\omega_i)f_r(p,\omega_i,\omega_o)\ \Large\frac{cos\theta\ cos\theta'}{||x'-x||^2}\normalsize \ dA$
    * 【应用了积分中的积分域变换，这里不太懂】

  ````
  shade(p, wo)
  	# Contribution from the light source.
  	Uniformly sample the light at x' (pdf_light = 1 / A)
  	Shoot a ray from p to x'
  	if the ray is not blocked in the middle
  		L_dir = L_i * f_r * cosθ * cosθ' / |x' - p|^2 / pdf_light
  	
  	# Contribution from other reflectors.
  	L_indir = 0.0
  	Test Ruassian Roulette with probability P_RR
  	Uniformly sample the hemisphere toward wi (pdf_hemi = 1 / 2pi)
  	Trace a ray r(p, wi)
  	If ray r hit a non-emitting object at q
  		L_indir = shade(q, -wi) * f_r * cosine / pdf_hemi / P_RR
  	
  	Return L_dir + L_indir
  ````

  * 直接光照部分，面光源采样点有很多？那要怎么判断光线是否被blocked

#### Some Side Notes

* Path tracing is indeed difficult
  * consider it the most challenging in undergrad CS
  * Why: physics, probability, calculus, coding
  * Learning PT will help you understand in these
* Ray tracing: Previous vs. Modern Concepts
  * Previous
    * Ray tracing == Whitted-style ray tracing
  * Modern
    * **The general solution of light transport**, including
      * (Unidirectional & bidirectional) path tracing
      * Photon mapping
      * Metropolis light transport
      * VCM / UPBP ...

#### Things we haven't covered / won't cover

* 如何在半球上均匀的采样？
  * 更通用的，如何采样一个函数？
  * sampling
* 蒙特卡洛积分如何应用在任意的pdf上？
  * importance sampling
* Low discrepancy sequences
* 如何把半球采样和光线采样结合起来？
  * multiple imp. sampling
* 每个像素的radiance是否简单平均起来就好了？要不要加权平均？
  * pixel reconstruction filter
* 如何把radiance转换为颜色？
  * gamma correction, curves, color space
* **Fear the science**
* 《我什么都不会》令闫琪

***

## Materials 材质

* What is Material in Computer Graphics ?
  * Material == BRDF

* Diffuse / Lambertian Material
  * $f_r = \Large\frac{\rho}{\pi}$
    * $\rho$: albedo 反射率(color)
* Glossy Material
* Perfect Specular Reflection
  * $\omega_o = -\omega_i +2(\omega_i\cdot\vec{n})\vec{n}$
  * $\phi_o=(\phi_i+\pi)\ mod\ 2\pi$
  * $\omega,\ \phi$ 是方位角
* **Refraction 折射**
  * Snell's Law
    * $\eta_i\ sin\theta_i = \eta_t\ sin\theta_t$
    * $cos\theta_t=\sqrt{1-(\Large\frac{\eta_i}{\eta_t}\normalsize)^2(1-cos^2\theta_i)}$
    * 当$\Large\frac{\eta_i}{\eta_t}\normalsize > 1$时，该式子可能无意义（即该反射为全反射）
  * Snell's Window / Circle
    * $97.2°$
    * ![17-1](pic\17-1.png)
* Fresnel Term
  * 解释光线中，有多少能量反射，多少能量折射
  *  S极化光线 和 P极化光线 的fresnel term不同
  * 入射光平行多反射；入射光垂直多折射；
  * ![17-2](/pic/17-2.png)

#### Microfacet

* Microfacet Theory
  * Rough Surface
    * Macroscale: flat & rough
    * Microscale: bumpy & **specular**
  * Individual elements of surface act like **mirrors**
    * Known as Microfacets
    * Each microfacet has its own normal
* Microfacet BRDF
  * 微表面的法线分布
    * 集中 <=> glossy
    * 分散 <=> diffuse
  * $f(i, o) = \Large \frac {F(i, h)\ G(i, o, h)\ D(h)}{4(n, i)\ (n, o)}\normalsize$
    * $F(i, h)$ : Fresnel term
    * $G(i, o, h)$ : shadowing-masking term
      * 修正入射角/出射角接近90°(Grazing Angle)的特殊情况
    * $D(h)$ : distribution of normals
* Isotropic / Anisotropic Materials 各向同性/各向异性 材质
  * Anisotropic: $f_r(\theta_i, \phi_i;\theta_r, \phi_r) ≠ f_r(\theta_i,\theta_r,\phi_r-\phi_i)$
  * Nylon尼龙：介于各向同性与各向异性之间
  * Velvet天鹅绒：可以人为地更改微表面，实现各向异性
* **Properties of BRDFs**
  * Non-negativity：函数值非负
  * Linearity：线性
  * Reciprocity principle：可逆性
    * $f_r(\omega_r\rightarrow\omega_i)=f_r(\omega_i\rightarrow\omega_r)$
  * Energy conservation：能量守恒

* MERL BRDF Database
  * 测量不同材质的数据的数据库

***

## Advanced Topics in Rendering

* unbiased 无偏

  * Monte Carlo积分在任何情况下均为1（无系统误差）

* biased 有偏

  * 期望值正确

* **Bidirectional Path Tracing** (BDPT)

  * 适合 "光源被遮罩，整个场景主要被间接光照亮" 的情况
  * 很难写。如果能自己实现BDPT，那自己实现渲染器就没什么问题了
  * ![18-1](pic\18-1.png)

* **Metropolis Light Transport** (MLT)

  * 利用Markov Chain根据旧采样样本，在旧样本周围生成新样本
  * 适合困难光路的场景
  * ![18-2](pic\18-2.png)
  * 缺点：无法算出图像收敛时间；渲染结果会"dirty"

* **Photon Mapping 光子映射**

  * 特别适合用来渲染 **Caustics**

  * ![18-3](pic\18-3.png)

  * 算法流程

    * Stage 1 photon tracing：从光源射出光子，一直弹射，直到打到diffuse表面

    * Stage 2 photon collection：从眼睛射出光路，一直弹射，直到打到diffuse表面

    * Calculation - local density estimation：

      ​	对每个渲染点，找最近的N个光子(BVH)，算出所占面积

  * Why biased?

    * 现象：产生模糊
    * $dN / dA != \Delta N / \Delta A$
    * 在N相同的情况下，光源射出光子数量越多，$ \Delta A$ 越接近 $dA$

  * An easier understanding bias in rendering

    * biased == blurry
    * consistent == con blurry with infinite #samples

  * 为什么不计算一定面积的光子数量

    * 结果不收敛，一定有偏，不再Consistent

* **Vertex Connection and Merging** (VCM)

  * 结合了双线路径追踪和光子映射
* **Instant Radiosity 实时辐射度** (IR)

  * 把已经被照亮的面积看作新的光源

***

## Advanced Appearance Modeling

* **Participating Media 散射介质**: Fog / Cloud
  * Phase Function 相位函数：决定光线如何散射
  * Participating Media: Rendering

* **Hair Appearance**
  * Kajiya-Kay Model: 不够优秀，效果还是很假
  * Marschner Model: 
    * 把头发当成玻璃圆柱，内部有不同颜色色素可以吸收颜色；
    * 光线分为三种：R/TT/TRT
  * **Double Cylinder Model**:
    * 闫老师发明的
    * Medulla 髓质: 髓质大小影响光线散射
    * 加入髓质，形成双层髓质模型
    * 光线分为五种：R/TT/TRT/TTs/TRTs
    * ![18-4](pic\18-4.png)
    * "为什么大家用了我的模型后，只能拿奥斯卡最佳视效提名，拿不到最终的奖呢？"
* **Granular Material** 颗粒材质
  * 一粒一粒的模型（晶体、糖）
* **Subsurface Scattering 次表面散射**
  * 应用于：Translucent Material 半透明材质
  * 在表面下散射，可以看作BRDF的延申
  * Dipole Approximation：将实际光源映射为虚拟光源
  * ![18-5](pic\18-5.png)
* **Cloth**
  * Render as Surface
  * Render as Participating Media
  * Render as Actual Fibers
* **Details**
  * 通过将pdf函数进行干扰，使得模型渲染后出现各种划痕
    * 模拟小晶点，也可以通过更改法线分布来实现
  * 金属的细节模拟，需要通过波动光学来计算pdf函数
    * “波动光学这事讲了要死人的”

* Procedural Appearance 程序化表面
  * 噪声函数

***

## Cameras

* Imaging = Synthesis + Capture

  * 捕捉成像：用相机拍照
* Cameras

  * Pinholes & Lenses：控制方向
  * Shutter 快门：控制光进入
  * Sensor 传感器：记录Irradiance信息
    * 有人在研究能区分不同方向光的传感器（能记录radiance）
  * 光线追踪基于针孔摄像机的模型
  * Field of View 视场
    * 针孔摄像机可以根据相似三角形来推导FOV
    * 大家计算时默认胶片高度35mm，通过改变焦距来改变FOV
      * 等效高度35mm，实际高度不一定
      * 17mm焦距 <=> wide angle 104°
      * 50mm焦距 <=> "normal" lens 47°
      * 200mm焦距 <=> telephoto lens 12°
    * ![19-1](pic\19-1.png)
    * ![19-2](pic\19-2.png)
* Exposure

  * Exposure = time * irradiance
    * ![19-3](pic\19-3.png)
  * Aperture size 光圈大小：改变 **F-Stop (F-Number)** 来控制光圈
    * F-Number：Written as FN or F/N；表示 光圈直径的乘法逆元；

  * Shutter speed 快门速度
    * 长快门时间会导致运动模糊 motion blur
    * Rolling Shutter：物体运动速度比快门速度快，会造成物体扭曲
  * ISO gain 感光度：可以理解成后期对能量乘上某个常数
    * 高ISO会导致噪声
  * High-Speed Photography
    * 快门速度极快 + 大光圈/高ISO
  * Long-Exposure Photography
    * 延时摄影，拉丝
* Thin Lens Approximation

  * $ \Large\frac{1}{f}\normalsize = \Large\frac{1}{z_o}\normalsize + \Large\frac{1}{z_i}\normalsize$
  * $f$ 焦距；$z_o$ object物距；$z_i$ image像距；
  * Defocus Blur
  * Circle of Confusion
    * 一个点经过透镜形成一个圆的像
    * $\Large\frac{C}{A} = \frac{d'}{z_i} = \frac{|z_s-z_i|}{z_i}$
  * $A$ 光圈直径
* Depth of Field 景深  
  * 景深 就是指 成像清晰的一段范围
  * ![19-4](pic\19-4.png)
  * 光圈越小，景深范围越大，清晰的范围越大，模糊效果越小

***

## Light Fields 光场

* Aesthetics (美学) is extremely important!
* Light Fields 光场 = Lumigraph 流明图
* The Plenoptic Function 全光函数
  * the set of all things that we can ever see.
  * $P(\theta, \phi)$ : 在一个位置上，向所有方向看到的东西
  * $P(\theta, \phi, \lambda)$ : 在一个位置上，向所有方向看到的东西和它们的颜色
  * $P(\theta, \phi, \lambda, t)$ : 在一个位置上，想所有方向看到的东西和它们的颜色，并且不同时间看到的东西不一样（电影）
  * $P(\theta, \phi, \lambda, t, V_x, V_y, V_z)$ : 在一个位置上，想所有方向看到的东西和它们的颜色，并且不同时间看到的东西不一样，并且可以改变观测的位置（全息电影 / 世界） 
* Light Fields 光场
  * Ray：5D -> 4D (在光场函数下)
  * 光场：在任何一个位置，往任何一个方向去的光的强度
    * 光场是全光函数的一个部分
    * 定义在物体表面
    * 用2个数表示"包围盒"上的一个点，用2个数表示一个方向
  * 已知一个物体的光场，可以得到任意位置任意角度观测物体得到的图像
  * 2个参数的坐标+2个参数的方向
    * -> 2个参数的坐标+2个参数的坐标（用两个平面上的两个点表示光场）
* Light Field Camera 光场摄像机
  * Lytro: founded by Prof. Ren Ng (UC Berkeley)
  * 特性：可以在后期调焦距和光圈大小
  * 原理：将成光平面后移，每个像素记录该像素中来自不同方向的光（光场）
    * 取不同方向的光线，相当于微移摄像机
  * 缺点：分辨率低；高成本(大胶片的高分辨率，微透镜的高精细)

***

## Color 颜色

* Spectral Power Distribution (SPD) 谱功率密度

  * 线性性质，两个光的谱功率密度拥有可加性

* 颜色 是 人的感知 (human perception)
  * 感官细胞 Photoreceptor Cells
    * Rods 棒状细胞：感知光的强度 (灰白)
      * ~120 millions rods in eye
    * Cones 锥形细胞：感知光的颜色
      * 分为 S-Cone、M-Cone、L-Cone
      * 三种锥形细胞分别感知不同波长的光线（S短 / M中 / L长 波长）
      * 不同人的眼睛中，三中锥形细胞的分布差别巨大
      * 人感知到的颜色，是SPD积分后的结果
  * Metamers 同色异谱现象
    * 同种颜色有着不同SPD
    * ![20-1](pic\20-1.png)
    * 可以将一种颜色的光谱进行转换成另一种相同颜色的光谱，便于显示器显示
  * 视觉暂留现象
    * 可以证明人体成像的补色

* Color Reproduction / Matching
  * Additive Color 加色系统
  * CIE RGB Color Matching
    * Red 700nm;
    * Green 546.1nm
    * Blue 435.8nm
    * CIE RGB 给出三原色的单波长SPD
  * Standardized RGB (sRGB)
  * Gamut 色域
    * 一种机制能显示的所有颜色
  * HSV Color Space
    * ![20-2](pic\20-2.png)
  * CMYK 减色系统

***

## Animation 动画 / Simulation 模拟

* Historical Points of Animation
  * 岩石壁画
    * Shahr-e Sukhteh, Iran 3200 BCE
  * 连贯的图像
    * Phenakistoscope, 1831
  * 用于生物学研究的早期电影
    * Edward Muybridge, "Sallie Gardner", 1878
  * 手绘动画 (First Hand-Drawn Feature-Length(>40min) Animation)
    * Disney，白雪公主和七个矮人, 1937
  * 数字动画
    * Ivan Sutherland, "Sketchpad", 1963 - Light pen, vector display
    * Ed Catmull & Frederick Parke, "Computer Animated Faces", 1972
  * CG (Computer Generation)
    * Jurassic Park, 1993
    * Toy Story, 1995
    * Cloudy With a Chance of Meatballs, 2009
    * Frozen 2, 2019
* Keyframe Animation
  * Keyframe Interpolation
  * Physical Simulation 
    * Clothes
    * Fluids
* Mass Spring System 质点弹簧系统
  * Example:
    * Mass Spring Rope
    * Hair
    * Mass Spring Mesh
  * A Simple Spring
    * Idealized spring
      * 长度为零
      * $f_{a\rightarrow b} = k_s(\vec b - \vec a)$
      * $f_{b\rightarrow a} = -f_{a\rightarrow b}$
    * Non-Zero Length Spring
      * $f_{a \rightarrow b} = k_s \Large\frac{\vec b - \vec a} {||\vec b - \vec a||}\normalsize(||\vec b - \vec a|| - l)$
      * 缺点：弹簧会无限震动下去(无摩擦力, 能量损失)
    * Simple motion damping
      * $f_b = -k_d\  \vec b'$
      * $f_{b} = -k_d\  \Large\frac{\vec b - \vec a} {||\vec b - \vec a||}\normalsize \cdot(\vec b' - \vec a')$
        * 绝对速度 -> 相对速度 -> 相对速度的投影
  * Structures from Springs
    * Sheets
    * Blocks
    * Others...
    * This structures will not resist shearing
    * This structures will not resist out-of-plane(非平面) bending
  * Improvements
    * pic/21-1
  * Aside
    * FEM (Finite Element Method) Instead of Springs
* Particle Systems 粒子系统
  *  把流体当作粒子
  *  For each frame in animation
     *  [If needed] Create new particles
     *  Calculate forces on each particle
     *  Update each particle's position and velocity
     *  [If needed] Remove dead particles
     *  Render particles
* Kinamatics 运动学
  * 正运动学：给出各个关节的状态，算出尖端位置
  * 逆运动学：给出尖端位置，反解出各个关节的状态









