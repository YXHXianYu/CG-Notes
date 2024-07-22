# Notes (Chap8-x)

## 8. Basics of Animation Technology

* Challenges in Game Animation
  * (1/3) Interactive and dynamic animation
    * 无法预测玩家行为
  * (2/3) Real-time
  * (3/3) Realism

### 8.1 2D Animation

* Sprite Animation
  * 用贴图循环播放
  * Sprite-like Animation in pseudo-3D game
    * ![image-20240224114412119](./Notes%20(Chap8-x)/image-20240224114412119.png)
  * Sprite Animation in Modern Game
* Live2D
  * 将人物的每个身体组件拆开，通过各种变换进行组合
  * 每个组件：深度 + 控制点 + 关键帧
  * ![image-20240224114600867](./Notes%20(Chap8-x)/image-20240224114600867.png)

### 8.2 3D Animation Techniques in Games

* DoF (Degrees of Freedom)
  * 对于一个刚体来说，空间中有6个DoF
    * translation x/y/z，rotation yaw/roll/pitch
* Rigid Hierarchical Animation
  * 刚体 + 层级
* Per-vertex Animation
  * 用预处理texture来记录每个顶点随时间变化的位置
  * 适用于：旗帜、水流
* Morph Target Animation
  * 调整多个预设动画的权重，通过插值来得到最终效果
* 3D Skinned Animation
  * 骨骼驱动动画
* 2D Skinned Animation
  * 2D的骨骼驱动动画
* Physical-based Animation
  * 适用于：Ragdoll，Cloth and Fluid Simulation，IK
* Animation Content Creation
  * 如何创造Animation：手K动画，Motion Capture

### 8.3 Skinned Animation Implementation

* Step
  * ![image-20240224120813448](./Notes%20(Chap8-x)/image-20240224120813448.png)
* Different Spaces
  * Local Space ∈ Model Space ∈ World Space
  * Local Space: 骨骼的坐标系
* Skeleton for Creature
  * Category：Humanoid Skeleton，Non-humanoid Skeleton
  * 对于Humanoid Skeleton来说，Pelvis Joint在脊椎的最下方的一个骨骼上，而Root Joint在Pelvis的正下方和脚底平面上
* Joint vs. Bone
  * ![image-20240224121700360](./Notes%20(Chap8-x)/image-20240224121700360.png)
  * Joint有自由度，Bone无自由度，会被Joint决定
* Humanoid Skeleton in Real Game
  * ![image-20240224121825447](./Notes%20(Chap8-x)/image-20240224121825447-1708748305872-1.png)
* Joints for Game Play
  * Weapon Joint，Mount Joint
  * 人物拿武器、骑乘，实际上都是把另一个模型绑定在某个特殊的joint上
* Pelvis Joint 和 Root Joint
  * root joint，在跳跃和蹲下的时候都起作用
  * ![image-20240224215251288](./Notes%20(Chap8-x)/image-20240224215251288.png)
  * ![image-20240224215235497](./Notes%20(Chap8-x)/image-20240224215235497.png)
* Bind Animation for Objects
  * 两个模型绑定在一起的时候，位置和方向都要一起变（比如汽车转向，人也转向）
    * 更类似于一个卡槽，方向也要变
* Bind Pose (T-Pose vs. A-pose)
  * T-Pose的问题，肩膀被挤压，所以现在行业中更常用A-Pose
* Skeleton Pose
  * Joint Pose共有9DoFs
    * Position，Orientation，Scale

### 8.4 Math of 3D Rotation

* 2D Orientation Math

* 3D Orientation Math - Euler Angle

  * 可以绕三个不同的轴旋转，也可以绕空间中任意一个轴旋转
  * 欧拉角，公式如下
    * ![image-20240224220211629](./Notes%20(Chap8-x)/image-20240224220211629.png)
  * 欧拉角的问题
    * **Order Dependency**
      * 旋转顺序不可变，先转x再转y ≠ 先转y再转x
    * **Gimbal Lock**
    * Hard to interpolate
    * Difficult for rotation combination
    * Hard to rotate by certain axis

* 3D Orientation Math - Quaternion

  * 四元数的原理只在二维和三维空间中成立
    * 原理和 “五次及五次以上的方程没有解析解” 有关
  * 四元数
    * $q = a + bi + cj + dk (a, b, c, d \in R)$
    * $i^2 = j^2 = k^2 = ijk = -1$
    * $\Rightarrow ij=k,\ ik=-1,\ etc..$
    * 四元数的Product
      * ![image-20240224222446353](./Notes%20(Chap8-x)/image-20240224222446353.png)
    * 四元数的Norm
      * 平方和再开根
    * 四元数的Conjugate（共轭）
      * $q^*=a-bi-cj-dk$
    * 四元数的Inverse
      * $q^{-1}q=qq^{-1}=1$
  * 四元数的应用
    * 用Quaternion表达旋转
      * 设旋转轴为 $\vec{u} = (u_x, u_y, u_z)$，旋转角为 $\theta$
      * 则四元数为 $q = cos(\frac{\theta}{2}) + u_xi\ sin(\frac{\theta}{2}) + u_yj\ sin(\frac{\theta}{2}) + u_zk\ sin(\frac{\theta}{2})$
    * 将向量 $\vec{v}$ 绕向量 $\vec{u}$ 旋转角度 $\theta$
      * 原向量Quaternion：$v' = 0 + v_x i + v_y j + v_z k$
      * 旋转Quaternion：$q = cos(\frac{\theta}{2}) + u_xi\ sin(\frac{\theta}{2}) + u_yj\ sin(\frac{\theta}{2}) + u_zk\ sin(\frac{\theta}{2})$
      * 旋转Quaternion的Conjugate：$q = cos(\frac{\theta}{2}) - u_xi\ sin(\frac{\theta}{2}) - u_yj\ sin(\frac{\theta}{2}) - u_zk\ sin(\frac{\theta}{2})$
      * 新向量Quaternion：$v_{rotated} = qv'q^*$
    * 注：旋转轴 $\vec{u}$ 必须为单位向量，这样旋转四元数才是单位四元数，从而保证旋转不会改变向量长度
    * 矩阵形式
      * 将 $v_{rotated} = qv'q^*$ 进行展开，并且按项进行合并，最后可以得到关于 $v_x, v_y, v_z$ 三个项的一个多项式，将其整理得到一个矩阵，即为四元数旋转的矩阵形式！
      * ![image-20240224225134435](./Notes%20(Chap8-x)/image-20240224225134435.png)
    * 反向旋转
      * 即为旋转四元数的Inverse

### 8.5 Joint Pose
Joint Pose的姿态

* Position
* Scale

## 9. Advanced Animation Technology

### 9.1 Animation Blending

* Animation Blending
  * ![image-20240329213309117](./Notes%20(Chap8-x)/image-20240329213309117.png)

* Blending Space

### 9.2 ASM (Animation State Machine)

* Animation State Machine
  * Case：Jumping —— ASM
  * ![image-20240329213419784](./Notes%20(Chap8-x)/image-20240329213419784.png)
  * ASM的Cross Fades
    * 各种动画的过渡应该是不一样的
    * 例如起跳，那么就不应该用平滑过渡等
* Layer ASM

### 9.3 ABT，Animation Blend Tree，动画树

* 动画树实际上适用于结合多种动画的，而不是做状态机的
  * ![image-20240329214046726](./Notes%20(Chap8-x)/image-20240329214046726.png)
* ABT是Layer ASM的超集，ABT是一个递归结构
