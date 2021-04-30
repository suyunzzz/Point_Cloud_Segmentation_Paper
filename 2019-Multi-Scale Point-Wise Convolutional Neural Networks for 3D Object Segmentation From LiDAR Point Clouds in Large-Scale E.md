# Multi-Scale Point-Wise Convolutional Neural Networks for 3D Object Segmentation From LiDAR Point Clouds in Large-Scale Environments

*Lingfei Ma , Student Member, IEEE, Ying Li , Jonathan Li , Senior Member, IEEE, Weikai Tan, Yongtao Yu , Member, IEEE, and Michael A. Chapman*

年份：2019

期刊：IEEE TRANSACTIONS ON INTELLIGENT TRANSPORTATION SYSTEMS

---

## 一、创新

> 提出一个多尺度、动态逐点卷积操作对点云进行特征提取。
>
> 与现存的方法相比，我们的方法对数据分布不那么敏感，且对计算资源要求不那么大。

1、出一个改进的point-wise卷积操作；

2、U形网络模型

3、使用自适应图卷积（KNN）实现high-level的边特征提取

4、条件随机场优化

![image-20210413114757754](https://gitee.com/suyunzzz/img/raw/master/img/20210413114757.png)

> 基本上就是先使用改进的PointConv进行特征提取，然后加一个edge-conv模块。

## 二、具体实现

### 1、PointCONV

对PointConv进行改进，主要是PointConv(cvpr2019)考虑的是在一个固定的密度尺度的近似，而本文考虑的是多尺度的密度估计。

![image-20210413143336749](https://gitee.com/suyunzzz/img/raw/master/img/20210413143336.png)

### 2、Edge-Conv

在PointCONV进行特征提取后，为了考虑点和其邻域点之间的边的特征，引入了Edge-Conv。在本文中，把Edge-Conv的求和函数替换为max函数，以减少了计算损失，并更换了边的更新函数为：

![image-20210413144120972](https://gitee.com/suyunzzz/img/raw/master/img/20210413144121.png)

### 3、Post-Processing：CRF

这部分不了解，没细看。



## 三、实验

### 1、Paris-Lille-3D

![image-20210413150849993](https://gitee.com/suyunzzz/img/raw/master/img/20210413150850.png)

![image-20210413150904642](https://gitee.com/suyunzzz/img/raw/master/img/20210413150904.png)

![image-20210413150910942](https://gitee.com/suyunzzz/img/raw/master/img/20210413150910.png)

### 2、scan-net

![image-20210413151430817](https://gitee.com/suyunzzz/img/raw/master/img/20210413151430.png)

### 3、S3DIS

![image-20210413151447812](https://gitee.com/suyunzzz/img/raw/master/img/20210413151447.png)