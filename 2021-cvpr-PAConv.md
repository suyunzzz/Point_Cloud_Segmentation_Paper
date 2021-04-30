# PAConv: Position Adaptive Convolution with Dynamic Kernel Assembling on Point Clouds

*Mutian Xu1* Runyu Ding1* Hengshuang Zhao2 Xiaojuan Qi1† 1The University of Hong Kong 2University of Oxford*

*cvpr-2021*

*github：https://github.com/CVMI-Lab/PAConv*

---

## 〇、Introduction

如何处理点云？主要可以分为两大类：

- voxelize

  > geometric information might be lost due to quantization, and voxels typically bring extra memory and computational costs

  - Voxnet: A 3d convolutional neural network for real-time object recognition. In IROS, 2015. 
  - SPLATNet: Sparse lattice networks for point cloud processing. In CVPR, 2018. 
  - Octnet: Learning deep 3d representations at high resolutions. In CVPR, 2017.

- PointNet-based

  - MLP系列

    > 使用的都是shared mlp，这限制了表达不同空间信息关系的能力
    >
    > they all adopt the shared MLPs to transform point features, which limits the model capabilities in capturing spatial-variant information.

    - PointNet
    - PointNet2
    - 3d graph neural networks for rgbd semantic segmentation. In ICCV, 2017.
    - Local spectral graph convolution for point set feature learnin. In ECCV,2018
    - A-cnn: Annularly convolutional neural networks on point clouds. In CVPR, 2019. 
    - Dynamic graph cnn for learning on point clouds. ACM Trans. Graph., 2019.

  - 卷积系列：探索局部邻域内空间关系

    > 基于相对空间信息直接预测卷积核权重
    >
    > 缺陷：much higher complexity (memory and computation)

    - Spidercnn:Deep learning on point sets with parameterized convolutional filters. In ECCV, 2018
    - Deep parametric continuous convolutional neural networks. In CVPR, 2018
    - RS-CNN . In CVPR 2019
    - Pointconv: Deep convolutional networks on 3d point clouds. In CVPR, 2019

    >卷积的另一个方向就是使用固定的卷积点，通过计算点之间的相关度来调整卷积核的权重
    >
    >limit the model flexibility

    - Point convolutional neural networks by extension operators. ACM Trans. Graph., 2018.
    - Kpconv: Flexible and deformable convolution for point clouds. In ICCV, 2019.
    - Interpoated convolutional networks for 3d point cloud understanding. In ICCV, 2019
    - Seggcn: Efficient 3d point cloud segmentation with fuzzy spherical kernel. In CVPR, 2020

  - 动态卷积、条件卷积。Dynamic and conditioned convolutions.

    > 本文与Condconv较为相识，但是Condconv预测的卷积核不是根据位置来的，但是在无序点云中需要根据空间位置来确定权重

    - Dynamic filter networks. In NeurIPS, 2016.
    - long-range interactions without attention ,2021
    - Condconv: Conditionally parameterized convolutions for efficient inference. In NeurIPS, 2019. 3



![几种方法的对比](https://gitee.com/suyunzzz/img/raw/master/img/20210430110909.png)



## 一、主要创新

1、通过**动态地**汇集权重矩阵构建一个卷积核，这些权重矩阵的参数是通过使用ScoreNet从点云的位置信息中学习到的。这样，卷积核就是通过**数据驱动**的方式构建的，与2dcnn相比更好处理不规则、无序的点云数据。

2、学习过程的复杂度从`根据点位置信息直接估计卷积核*`降低到 `估计系数来联合权重矩阵`。

3、以往的方法的网络架构都是高度订制的，很难清楚是module的作用还是网络结构的作用，本文将PAConv集合到MLP-based的框架中，不改变网络配置，达到了sota的效果。





## 二、具体实现

![image-20210430144310575](https://gitee.com/suyunzzz/img/raw/master/img/20210430144310.png)



以二维为例，2d卷积可以描述为：

![image-20210430111416963](https://gitee.com/suyunzzz/img/raw/master/img/20210430111417.png)

其中，N代表邻域（3x3），f_j代表每一点的输入特征C_in维，K代表一个权重矩阵，将C_in维特征映射为C_out维，A代表邻域特征聚合操作。假如3x3的邻域，那么就有9个C_in*C_out的映射矩阵。

![2dcnn](https://gitee.com/suyunzzz/img/raw/master/img/20210430113738.png)

但是由于点云是无序的，不规则的，在一个点的邻域内的点的位置是不确定的（图像指定一个3x3的邻域，其点的数量是固定的），因此在点云的邻域内不能得到有限个权重矩阵。

该问题的解决如下：

### 1、Weigh Bank

定义一个矩阵集合B，每一个矩阵是C_in*C_out的大小，该集合共有M个矩阵

![image-20210430114113096](https://gitee.com/suyunzzz/img/raw/master/img/20210430114113.png)

最后得到的每一个邻域点的权重矩阵（C_in*C_out）就是这M个矩阵的的加权和。





### 2、ScoreNet

![image-20210430144210543](https://gitee.com/suyunzzz/img/raw/master/img/20210430144210.png)

对每一个邻域点p_j，计算其与中心点p_i的相对距离，根据空间位置信息，得到一个M维的分数向量，代表M个矩阵的加权系数。

![image-20210430114909673](https://gitee.com/suyunzzz/img/raw/master/img/20210430114909.png)

其中S_ij是M个系数的集合，代表j这个点上的M个weigh matrix的权重系数。

这样就建立了一个从离散的卷积核映射到连续的三维空间的映射

> 这里连续的三维空间是怎么体现的呢？
>
> 我的理解是根据距离，距离连续的，无论什么距离，都可以得到一组分数作为weight matrix的加权系数。





### 3、Kernel generation

![image-20210430144223701](https://gitee.com/suyunzzz/img/raw/master/img/20210430144223.png)

根据以上，p_j点处的卷积权重就是：

![image-20210430115051379](https://gitee.com/suyunzzz/img/raw/master/img/20210430115051.png)

这样，p_j的卷积核权重就是根据空间信息动态获取的了，这种位置自适应的卷积对于不规则分布的点云有很高的的灵活性。



### 4、Weight Regularization

因为权重矩阵需要代表不同类型的信息，为了使M个权重矩阵尽可能地具有差异性，引入了一个Loss函数来惩罚他们的相识性：

![image-20210430115755950](https://gitee.com/suyunzzz/img/raw/master/img/20210430115756.png)

这使得权重矩阵尽可能多样分布，进而保证了卷积核的多样性。

> 关于这个式子的理解，可以简单理解成是计算余弦相识度，参考两个向量的内积，这里就是量量计算weigh matrix的相识度，然后求和

## 三、实验

### 1、分类

> 任务简单，没有引入L_corr函数

![image-20210430142317437](https://gitee.com/suyunzzz/img/raw/master/img/20210430142317.png)

### 2、部件分割

![image-20210430142432012](https://gitee.com/suyunzzz/img/raw/master/img/20210430142432.png)

### 3、场景分割

![image-20210430142454185](https://gitee.com/suyunzzz/img/raw/master/img/20210430142454.png)

![image-20210430144018908](https://gitee.com/suyunzzz/img/raw/master/img/20210430144019.png)

![image-20210430144030210](https://gitee.com/suyunzzz/img/raw/master/img/20210430144030.png)

![image-20210430144104556](C:\Users\11604\AppData\Roaming\Typora\typora-user-images\image-20210430144104677.png)

### 4、消融实验

> 在场景分割下进行

#### 1、ScoreNet

输入

![image-20210430142656153](https://gitee.com/suyunzzz/img/raw/master/img/20210430142656.png)



Score normalization

![image-20210430142726053](https://gitee.com/suyunzzz/img/raw/master/img/20210430142726.png)

可视化ScoreNet的输出

> 每一个点输出一个M维的Score
>
> 更好地表示方法：可是一个用M个点云来表示，每个点云代表对每个Weight Matrix的响应，点云的颜色越深代表这个点对该Weight Matrix的响应越大。

![image-20210430142818167](https://gitee.com/suyunzzz/img/raw/master/img/20210430142818.png)

Score distribution in the network

> 代表4个encoder层中，每一层对M（16）个weight matrix的响应的平均值
>
> 因为在i的邻域内，一个邻域点j会生成一个M维度的Score向量，求他们的平均值，就得到了这个折线图

![image-20210430143714566](https://gitee.com/suyunzzz/img/raw/master/img/20210430143714.png)



#### 2、The Number of Weight Matrices

![image-20210430143130790](https://gitee.com/suyunzzz/img/raw/master/img/20210430143130.png)

#### 3、Weight Bank Regularization

![image-20210430143249183](https://gitee.com/suyunzzz/img/raw/master/img/20210430143249.png)

#### 4、Robustness Analysis

![image-20210430143343224](https://gitee.com/suyunzzz/img/raw/master/img/20210430143343.png)