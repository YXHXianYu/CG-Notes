# 2024年Radiometry复习

* Radiant Energy
  * $Q [J = Joule]$
* Flux (Power)
  * $\Phi \equiv \frac{dQ}{dt}\ [W = Watt]\ [lm = lumen]^*$
* Radiant Intensity
  * Definition: **the Power per unit solid angle**
  * $I(\omega) \equiv \frac{d\Phi}{d\omega}$
* Solid Angle
  * 二维情况下，角度 $\theta = \frac{l}{r}$，圆有 $2\pi\ radians$
  * 三维情况下，立体角 $\Omega = \frac{A}{r^2}$，球有 $4\pi\ steradiance$
  * 单位立体角（微分立体角 Defferential Solid Angles）
    * 设 $\theta \in [0, 2\pi], \phi \in [0, \pi]$
    * 则 $dA = r^2\ sin\phi\ d\theta\ d\phi$
    * 则 $d\omega = sin\phi \ d\theta \ d\phi$
    * 单位球面面积 $dA = r^2 \ d\omega$
* Irradiance
  * Definition: **the Power per unit area**
  * $E(x) \equiv \frac{d\Phi(x)}{dA_⊥}$
    * 注：这里的 $dA_⊥$ 是单位面积，不同于之前的单位球面面积 $dA$
  * 比如，在光线向外传播，衰减时，实际就是Irradiance在变化（Intensity不变，因为半径增大，不影响单位立体角！）
    * ![image-20240913110958198](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913110958198.png)
* Radiance
  * Definition: **the Power, per Unit Solid Angle, per Projected Unit Area**
  * $L(p, \omega) \equiv \Large \frac{d^2\Phi(p, \omega)}{d\omega\ dA\ cos\theta}$ $\equiv \Large \frac{dE(p)}{d\omega\ cos\theta}$ $\equiv \Large \frac{dI(\omega)}{dA\ cos\theta}$
* Irradiance vs. Radiance
  * Irradiance: 小范围 $dA$ 内的所有能量
  * radiance: 小范围 $dA$ 内某个方向 $d\omega$ 上所有能量
  * $dE(p, \omega) = L_i(p, \omega)\ cos\theta \ d\omega$
    * 【这个式子怎么来的？】
  * $E(p) = \int_{H^2}L_i(p,\omega)\ cos\theta\ d\omega$
* BRDF
  * Definition: 一个比例，对于任何一个出射方向 $\omega_i$ 的radiance $dL_r(\omega_r)$ 除以 一个微小面积 $dA$ 接收到的irradiance $dE_i(\omega_i)$
    * 【为什么irradiance有方向？】
      * 我的理解：irradiance物理量是没有方向的，但是我们让irradiance对立体角 $d\omega$ 进行微分，微分后得到的 $dE$ 就有方向了！
      * 合理的！
  * $f_r(\omega_i \rightarrow \omega_r) = \Large \frac{dL_r(\omega_r)}{dE_i(\omega_i)} \normalsize = \Large \frac{dL_r(\omega_r)}{L_i(\omega_i)\ cos\theta_i\ d\omega_i}$
  * 【我的理解】
    * 因为BRDF中的irradiance是一个微分，所以BRDF中的irradiance是有方向的
    * 同时，irradiance对方向微分后，实际上就可以转化为radiance，即 $dE(p, \omega) = L_i(p, \omega)\ cos\theta \ d\omega$
  * 根据上式，我们就可以得到 **The Reflection Equation**
    * 变换再积分
    * ![image-20240913114311678](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913114311678.png)
  * 每个不同材质，就是定义这个 $f_r$
* The Rendering Equation
  * ![image-20240913115602758](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913115602758.png)
* Monte Carlo Integration
  * ![image-20240913121507361](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913121507361.png)
* Path Tracing
  * 假设一个材质均匀在单位半球上采样，那么每次radiance采样的pdf就是1/2pi
  * ![image-20240913122356332](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913122356332.png)
* 直接光照版
  * ![image-20240913122740575](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913122740575.png)
* 全局光照版
  * ![image-20240913123027655](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913123027655.png)
* 优化后：① 去掉for ② RR
  * ![image-20240913123849970](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913123849970.png)
* 采样BRDF vs. 采样光源
  * 如何采样光源？
  * ![image-20240913124234520](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913124234520.png)
  * ![image-20240913124306628](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913124306628.png)
    * 对光源的积分
* 新的shade函数
  * ![image-20240913124553240](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913124553240.png)
  * 记得判断光源是否被遮挡
* Lambertian BRDF推导
  * 假设：材质均匀（即$L_i$在各个方向$\omega_i$相同）、能量守恒（即$L_o <= \Sigma{L_i}$）
  * 推导：![image-20240913162452360](./2024%E5%B9%B4Radiometry%E5%A4%8D%E4%B9%A0/image-20240913162452360.png)
  * 最后，$f_r = \Large \frac{\rho}{\pi}$，其中 $\rho <= 1$
* 先去实现一下上面的代码，上面代码和RTIOW不同
  * 缺：重要性采样、MIS