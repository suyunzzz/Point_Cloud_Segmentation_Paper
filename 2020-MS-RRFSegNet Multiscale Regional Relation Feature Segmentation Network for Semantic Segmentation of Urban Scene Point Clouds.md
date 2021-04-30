# 2020-MS-RRFSegNet: Multiscale Regional Relation Feature Segmentation Network for Semantic Segmentation of Urban Scene Point Clouds

*时间：2020*

*期刊：IEEE Transactions on Geoscience and Remote Sensing*

*作者：Luo, Haifeng、Chen, Chongcheng、Fang, Lina、Khoshelham, Kourosh、Shen, Guixi*

*地址：https://github.com/Megasister/MS_RRFSegNet*

---

## 一、创新

> 为了解决点云的无序性、不均匀性、大场景的点云计算消耗大的问题，提出了一种先对点云过分割得到超体素，然后对超体素进行语义分割的方法。

![image-20210311113819428](https://gitee.com/suyunzzz/img/raw/master/img/20210311113819.png)

## 二、具体实现

### 1、Supervoxels and Embedding Representation

使用超体素作为基本的处理单元，不但减少了计算复杂度解决了点云分布的不均衡问题，而且有效地扩大了感受野以获取更丰富的上下文信息。

提出一种超体素自动编码策略，对超体素内的点云进行特征编码，将重建的损失函数作为优化对象：

![image-20210311170703900](https://gitee.com/suyunzzz/img/raw/master/img/20210311170703.png)





### 2、RRFRM（Region Relation Feature Represent Module）

通过构建中心超体素与其邻域超体素之间的图关系，将邻域超体素的特征嵌入到中心超体素上：

![image-20210311200436631](https://gitee.com/suyunzzz/img/raw/master/img/20210311200436.png)



### 3、MS-RRFSegNet

![image-20210311203525869](https://gitee.com/suyunzzz/img/raw/master/img/20210311203525.png)

未解决样本的极度不均衡问题，所使用的loss函数（加权交叉熵损失）如下：

![image-20210311203550751](https://gitee.com/suyunzzz/img/raw/master/img/20210311203550.png)

其中：

![image-20210311203945209](https://gitee.com/suyunzzz/img/raw/master/img/20210311203945.png)

![image-20210311204004059](https://gitee.com/suyunzzz/img/raw/master/img/20210311204004.png)



## 三、实验

NPM3D

![image-20210311205934587](https://gitee.com/suyunzzz/img/raw/master/img/20210311205934.png)



Semantic3d

![image-20210311205942611](https://gitee.com/suyunzzz/img/raw/master/img/20210311205942.png)