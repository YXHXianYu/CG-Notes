# Advanced Light Transport

* Biased vs. Unbiased（有偏vs无偏）
  * 无偏估计：估计值 = 期望
    * 例子：蒙特卡洛
  * 有偏估计：估计值 = 期望 + 偏差
    * 一致（Consistent）的有偏估计：在样本无穷多时，结果会收敛到估计值
    * 例子：光子映射

## 1. Unbiased light transport methods

### 1.1 Bidirectional Path Tracing (BDPT)

* 字面意思
* 特性：实现难度大、适合小光源/光源隐蔽的场景、速度慢

### 1.2 Metropolis Light Transport (MLT)

* 是一个 马尔科夫链 蒙特卡洛 的应用（Markov Chain Monte Carlo）
* 感性理解：根据前几个随机样本，生成下一个样本。（有几个路径的情况下，生成一条新的路径）
* 特性：适合做复杂光路的场景
  * ![image-20240918155355494](./2024%E5%B9%B4AdvancedLightTransport%E5%A4%8D%E4%B9%A0/image-20240918155355494.png)
  * 【2024思考：现在光路是不是可以用机器学习直接学，不一定需要随机生成了】
  * 注：为什么caustics难做？因为caustics是一个SDS的光路，SDS指的是Specular+Diffuse+Specular，要让光现在Diffuse反射后再通过Specular打到同一个点，这个概率非常低，所以PT难做。
  * 注：Caustics，即 由于光线 **聚焦** 导致一些非常强烈的图案
* 缺点：复杂度难以计算，无法确定收敛时间。正常情况下，可能会导致图形中同时存在很多收敛和不收敛的区域，会导致画面比较“脏”

## 2. Biased light transport methods

### 2.1 Photon Mapping (PM)

* 特性：biased method、two-stage method、适合用于渲染caustics
* Photon Mapping有许多实现方法，这里介绍其中一种：
  * Stage 1 —— Photon Tracing
    * 从光源发射光子，并不断追踪，直到光子击中一个diffuse表面，并在该diffuse表面记录光子
  * Stage 2 —— Photon Collecting
    * 从摄像机出发，不断追踪，直到追踪到一个diffuse表面。在这个表面上取临近的K个光子，记录所需面积
* 渲染特性
  * 当光子数量很少时，画面噪声严重；光子数量增多时，噪声减小，但是画面模糊
  * 原因是，Photon Mapping做了一个近似，假设 一个单位面积内的光子数量 = 临近K个光子 / 所占面积。而这里的面积不是无限小，所以是biased的
  * 但是，如果无限增大光子数量，那么这个所占面积就可以趋于无穷小，这个时候，结果仍然会正确，所以是consistent的
  * 所以，Photon Mapping是一个biased且consistent的积分器

* <img src="./2024%E5%B9%B4AdvancedLightTransport%E5%A4%8D%E4%B9%A0/image-20240922154245172.png" alt="image-20240922154245172" style="zoom:50%;" />
* 一个好问题：为什么不确定一个小范围，然后数里面的光子数量呢？
  * 因为这会导致PhotonMapping是biased，但不是consistent
  * 因为就算你有无限多个光子，你也仍然会有一个不是趋于无穷小的面积


### 2.2 Vertex Connection and Merging (VCM)

* 想法：双向路径追踪 + Photon Mapping

## 3. Instant radiosity (IR)

* 别名：many-light approaches、virtual point light（VPL）
* Key Idea：把已经被照亮的表面看作光源（虚拟点光源，VPL）
