# Vectors

### Vector Normalization

​	Unit Vector

***

### Vector Addition

​	Geometrically : Parallelogram law & Triangle law

​	Algebraically : Simply add coordinates

***

### Cartesian Coordinates

***

### Vector Multiplication

#### Dot Product

$$
\vec{a}\cdot\vec{b} = ||\vec{a}||\ ||\vec{b}||\ \cos(\theta)\\
\cos(\theta)=\tfrac{\vec{a}\cdot\vec{b}}{||\vec{a}||\ ||\vec{b}||}
$$

functions:

​	1.finding angle between  two vectors

​	2.finding projection of one vector on another

​	3.measure how close two directions are

​	4.decompose a vector

​	5.determine forward / backward

#### Cross product

$$
长度定义：||\vec{a}\times\vec{b}||=||\vec{a}||\ ||\vec{b}||\sin(\phi)\\
方向定义：右手螺旋定则：四指由\vec{a}旋转向\vec{b}，大拇指方向即结果向量方向\\
交换律: \vec{a}\times\vec{b}=-\vec{b}\times\vec{a}\\
\vec{a}\times\vec{a}=0
$$

functions:

​	1.Determin left / right (positive -> right)

​	2.Determin inside / outside 

#### Orthonormal Bases / Coorninate Frames

***

# Matrices

### What is Matrices ?

***

### Matrix-Matrix Multiplication

$$
(M\times N) (N \times P) = (M \times P)\\
c[i,j] = \sum_{1\le k\le N}
(a[i,k]\times b[k,j])
$$

### Matrix-Vector Multiplication

* Treat vector as a column matrix (m * 1)

### Transpose of a Matrix

* Switch rows and columns

### Identity Matrix and Inverses

$$
I_{3\times 3} = \left (
\begin{array}{ccc}
1 & 0 & 0\\
0 & 1 & 0\\
0 & 0 & 1\\
\end{array}\right)
\\
AA^{-1} = A^{-1}A = I\\
(AB)^{-1}=B^{-1}A^{-1}
$$

### Vector multiplication in Matrix from

* Dot product
* Cross product

