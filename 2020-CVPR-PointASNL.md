# PointASNL: Robust Point Clouds Processing using Nonlocal Neural Networks with Adaptive Sampling

期刊：CVPR2020

时间：2020

code：https://github.com/yanx27/PointASNL

## 创新

- 针对fps采样对噪声敏感的问题，提出自适应采样方法，看起来其实就是**一个局部特征丰富模块**
- 受non-local的启发，使用全局范围内的上下文信息，对采样点的特征进行增强

### 1、Adaptive Sampling（AS）模块

![image-20210112171317171](https://gitee.com/suyunzzz/img/raw/master/img/20210112171317.png)

对fps采样后的点，先对其邻域使用***self-attention***，更新邻域内点的信息。然后对增强后的邻域，在邻域点维度上（就是对每一个邻域点计算一个权重，用来更新采样点的特征和坐标）使用***attention***机制，然后加权求和得到更新后的采样点特征和坐标







### 2、local-nonlocal

![image-20210112171306009](https://gitee.com/suyunzzz/img/raw/master/img/20210112171306.png)

- local模块类似于**PointConv**的局部特征提取



- nonlocal模块本质上还是使用了`self-attention`（应该是一种特殊的attention，和self-attention很像），使用了*上一层的点云和采样后点云计算相识度*。

  ![image-20210112171436751](https://gitee.com/suyunzzz/img/raw/master/img/20210112171436.png)

  

## 具体实现

![image-20210112171652080](https://gitee.com/suyunzzz/img/raw/master/img/20210112171652.png)



## 实验结果

![image-20210112171852176](https://gitee.com/suyunzzz/img/raw/master/img/20210112171852.png)

![image-20210112171942608](https://gitee.com/suyunzzz/img/raw/master/img/20210112171942.png)

