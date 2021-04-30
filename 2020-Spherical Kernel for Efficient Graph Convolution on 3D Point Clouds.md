# Spherical Kernel for Efficient Graph Convolution on 3D Point Clouds

*Huan Lei, Naveed Akhtar, and Ajmal Mian*

---

提出一个球状卷积算子，该卷积操作保证了平移和旋转不变性以及不对称性质，前者保证了在一个相似的局部结构中权重共享，后者促进了几何学习。

---



# 一、创新 

1、在工作中分别执行了depth-wise和point-wise的卷积，而不是同时执行

2、使用图结构来构建网络，这允许我们开发新的卷积模块以及定义池化/上采样操作。和基于卷积的上采样下采样相反，本文的模块更容易处理更大数量的点云。这也使得我们的网络更接近于标准的CNN。



# 二、具体实现



# 三、实验