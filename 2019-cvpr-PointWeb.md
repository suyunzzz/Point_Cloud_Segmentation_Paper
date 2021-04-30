# PointWeb: Enhancing Local Neighborhood Features for Point Cloud Processing

*cvpr 2019*

*Hengshuang Zhao*

*地址：https://github.com/hszhao/PointWeb*

---

## 1、创新

针对之前的方法（PointNet、dgcnn的局部点云表达弱的问题，提出了一种考虑局部内所有点之间作用的module。

![image-20210310182030417](https://gitee.com/suyunzzz/img/raw/master/img/20210310182030.png)

## 2、具体表现

主要就是提出一个Adaptive Feature Adjustment（AFA）模块：

> 通过计算局部内所有点之间的影响来获得特征，更好地利用了邻域信息，有点类似于2020发表在模式识别上的这篇文章《Point attention network for semantic segmentation of 3D point clouds》，都是开始的时候考虑所有邻域点的影响来更新中心点。

![image-20210310183127641](https://gitee.com/suyunzzz/img/raw/master/img/20210310183127.png)

 

中心点的特征更新为：

<img src="https://gitee.com/suyunzzz/img/raw/master/img/20210310183234.png" alt="image-20210310183234463" style="zoom:50%;" />

也就是下式：

<img src="https://gitee.com/suyunzzz/img/raw/master/img/20210310183315.png" alt="image-20210310183315881" style="zoom:50%;" />

最终整理就会得到：

<img src="https://gitee.com/suyunzzz/img/raw/master/img/20210310183613.png" alt="image-20210310183613459" style="zoom:50%;" />

其中：<img src="https://gitee.com/suyunzzz/img/raw/master/img/20210310183628.png" alt="image-20210310183628231" style="zoom:50%;" />

网络结构如下：

![image-20210310183738192](https://gitee.com/suyunzzz/img/raw/master/img/20210310183738.png)

## 3、实验

只关注了S3DIS数据集的表现：

![image-20210310183805250](https://gitee.com/suyunzzz/img/raw/master/img/20210310183805.png)

