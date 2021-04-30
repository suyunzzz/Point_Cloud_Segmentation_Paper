# Efficient Point Cloud Convolutional Neural Networks using Concentric Shells Statistics

*Zhiyuan Zhang1 Binh-Son Hua2 Sai-Kit Yeung3 1Singapore University of Technology and Design 2The University of Tokyo 3Hong Kong University of Science and Technology*

---

*github：https://github.com/hkust-vgd/shellnet*

# 一、创新

1、提出ShellConv这一卷积模块，将一个邻域根据若干个相同质心的球壳分开，解决了高效的邻域查询，通过由里到外的的卷积顺序解决了点的顺序问题。

2、提出ShellNet，基于ShellConv直接学习3D点云，而不包含顺序奇异性。

3、在分类、部件分割、场景分割上去得了SOTA的结果（2019）

![image-20210409103247053](https://gitee.com/suyunzzz/img/raw/master/img/20210409103247.png)







# 二、具体实现

## Convolution

定义质心处的p的特征表示为：

![image-20210409111430296](https://gitee.com/suyunzzz/img/raw/master/img/20210409111430.png)

也就是我们常见的邻域内特征的累计求和，这里是以邻域点为单位，但是每一个点的w权重是不知道的（PointConv是根据该点处的密度来计算权重的）。

一个思路是给定一个固定尺寸的可训练参数，但是直接对点给定的话，是不行的，因为点集的顺序是变化的。

因此引入了多尺度的球壳，这样就以球壳shell为单位，对一个质心点p进行特征提取：

![image-20210409112306833](https://gitee.com/suyunzzz/img/raw/master/img/20210409112306.png)

> 由于这些shell是很自然地有顺序的（从最内层到最外层），这样对于球壳的顺序就没有奇异性了。
>
> 对于球壳内部点的顺序奇异性，采用了最大值函数：
>
> ![image-20210409112606213](https://gitee.com/suyunzzz/img/raw/master/img/20210409112606.png)

算法的结构如下：

![image-20210409144127575](https://gitee.com/suyunzzz/img/raw/master/img/20210409144127.png)

## Spherical Shells Construction

对每一个shell设置一个固定值，一个shell存放n个最近的点。

这种结构使得shell中点的分布变得均衡。

## 网络结构

![image-20210409154556484](https://gitee.com/suyunzzz/img/raw/master/img/20210409154556.png)





# 三、实验

![image-20210409155746674](https://gitee.com/suyunzzz/img/raw/master/img/20210409155746.png)