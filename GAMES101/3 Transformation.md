# Why study transformation

# 2D transformations

#### Scale Matrix

$$
\begin{bmatrix}
x^{'}\\y^{'}
\end{bmatrix}
=
\begin{bmatrix}
s_x & 0\\0 & s_y
\end{bmatrix}
\begin{bmatrix}
x\\y
\end{bmatrix}
$$

#### Reflection Matrix

$$
\begin{bmatrix}
x^{'}\\y^{'}
\end{bmatrix}
=
\begin{bmatrix}
-1 & 0\\0 & 1
\end{bmatrix}
\begin{bmatrix}
x\\y
\end{bmatrix}
$$

#### Shear Matrix

![3-3](pic\3-3.png)
$$
\begin{bmatrix}
x^{'}\\y^{'}
\end{bmatrix}
=
\begin{bmatrix}
1 & a\\0 & 1
\end{bmatrix}
\begin{bmatrix}
x\\y
\end{bmatrix}
$$

#### Rotate

* 我直接自己推出来了，耶！不愧是我 (✿◡‿◡)
  * "由特殊到一般"

$$
\mathbf{R_\theta} =
\begin{bmatrix}
\cos\theta&-\sin\theta\\
\sin\theta&\cos\theta
\end{bmatrix}
$$

* $\mathbf{R_{-\theta}}=\mathbf{R^{T}_{\theta}}$
* $\mathbf{R_{-\theta}}=\mathbf{R^{-1}_{\theta}}\ (by\ \ definition)$

#### Linear Transform

***

# Homogeneous coordinates

#### Translation

* Translation cannot be represented in matrix form

$$
\begin{bmatrix}
x^{'}\\y^{'}
\end{bmatrix}
=
\begin{bmatrix}
a & b\\c & d
\end{bmatrix}
\begin{bmatrix}
x\\y
\end{bmatrix}
+
\begin{bmatrix}
t_x\\t_y
\end{bmatrix}
$$

#### Solution: Homogenous Coordinates

* Add a third coordinate (w-coordinate)

  * 哇我自己想出来了欸233

* 2D point = $(x,y,1)^T$

* 2D vector = $(x,y,0)^T$

  * 向量的平移不变性
  * 同时还满足的点和向量加减法的性质问题，太妙了啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊

* In homogeneous coordinates,

  $\left( \begin{array}{c} x\\y\\w \end{array} \right) is\ the\ 2D\ point\left( \begin{array}{c} x/w\\y/w\\1 \end{array} \right), w \ne 0$

#### Affine Transformations

#### Inverse Transform

* 只要经过逆矩阵的变换就好了，太牛逼了

#### Composite Transform

* 一个3*3矩阵可以表示极端复杂的变换

#### Decomposing Complex Transforms

![3-4](pic\3-4.png)

# 3D Transformations

