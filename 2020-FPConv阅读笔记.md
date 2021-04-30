# FPConv: Learning Local Flattening for Point Convolution

cvpr2020

将已有的点云学习方式分为

- Volumetric-style point convolution
- Graph-style point convolution
- Surface-style point convolution

## 创新

提出一种基于表面的学习方式，通过学习一个权重矩阵，将采样点的的邻域点映射为一个平面，再使用2D CNN进行局部特征聚合。

![image-20210107095703275](https://gitee.com/suyunzzz/img/raw/master/img/20210107095703.png)

## 具体实现

![image-20210107095746223](https://gitee.com/suyunzzz/img/raw/master/img/20210107095746.png)

现将局部点云使用PointNet提取特征，然后学习一个**映射矩阵**，再使用2d cnn进行特征聚合。

对于大场景的分割问题设计如下网络结构：

![image-20210107100527907](https://gitee.com/suyunzzz/img/raw/master/img/20210107100527.png)

## 实验结果

可以和一些网络（volumetric-style）相互补充





![image-20210107100827748](https://gitee.com/suyunzzz/img/raw/master/img/20210107100827.png)



在s3dis上，FPConv对简单结构表现良好，KPConv对复杂结构表现良好

![image-20210107102948582](https://gitee.com/suyunzzz/img/raw/master/img/20210107102948.png)