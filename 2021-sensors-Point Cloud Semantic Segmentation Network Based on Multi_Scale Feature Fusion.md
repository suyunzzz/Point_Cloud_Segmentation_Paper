# Point Cloud Semantic Segmentation Network Based on Multi-Scale Feature Fusion

*Jing Du 1 , Zuning Jiang 1 , Shangfeng Huang 1 , Zongyue Wang 1 , Jinhe Su 1 , Songjian Su 2 , Yundong Wu 1,3 and Guorong Cai* 

期刊：sensors

年份：2021

作者说会开源



---



![image-20210318155805840](https://gitee.com/suyunzzz/img/raw/master/img/20210318155805.png)



## 一、创新

提出**多尺度特征特征融合**来增强有一个点云的在不同密度下的特征，提高了语义分割的性能。





## 二、具体实现

### Multiscale Point Feature Extraction

对点云进行两个分辨率的随机采样，分别记为P1，P2，然后对两个点云使用SAN(多方向卷积操作)进行特征提取。

> Spatial Aggregation Net具体可以见之前的文章（Spatial Aggregation Net: Point Cloud Semantic Segmentation Based on Multi-Directional Convolution）

### Feature Fusion and Loss Function

> 就特征融合的方式来讲。基于距离的的加权融合会导致不稳定的分割结果，因此作者提出直接映射的方式。

![image-20210318165009706](https://gitee.com/suyunzzz/img/raw/master/img/20210318165009.png)

![image-20210318165620568](https://gitee.com/suyunzzz/img/raw/master/img/20210318165620.png)

损失函数主要分为四部分：

![image-20210318170715570](https://gitee.com/suyunzzz/img/raw/master/img/20210318170715.png)

![image-20210318171005471](https://gitee.com/suyunzzz/img/raw/master/img/20210318171005.png)

![image-20210318172325926](https://gitee.com/suyunzzz/img/raw/master/img/20210318172325.png)



## 三、实验



#### S3DIS

In the experiments described below, **16,384 points were randomly selected from each sample**. For the first level of the network framework (Level-1), 8192 points were used as input, and for the second level (Level-2), 4096 points were used. The output consists of the classification results for these 4096 points.

定量比较仅仅比较了Acc，而没有比较mIoU

![image-20210318172803247](https://gitee.com/suyunzzz/img/raw/master/img/20210318172803.png)