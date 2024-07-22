# 3D transformations

* Rotation around x-, y-, or z-axis

* y轴旋转，由于叉乘定义，和正常旋转矩阵不同
* roll, pitch, yaw
* Rodrigues' Rotation Formula
  * ![4-1](pic\4-1.png)

# Viewing transformation

* View / Camera Transformation
* MVP Transformation (model view projection)
* Define the camera
  * Position $\vec{e}$
  * Look-at / gaze direction $\hat{g}$
  * Up direction $\hat{t}$
* Transform the camera to
  * The origin, up at Y, look at -Z
  * And transform the objects along with the camera
  * $M_{view}=R_{view}T_{view}$
  * 