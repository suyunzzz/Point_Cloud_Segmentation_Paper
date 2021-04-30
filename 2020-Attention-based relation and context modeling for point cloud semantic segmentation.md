# Attention-based relation and context modeling for point cloud semantic segmentation

*作者：Hu, Zhiyu*

*年份：2020*	

*期刊：Computers & Graphics*

---

## 一、创新

1、基于注意力的局部关系学习模块

2、上下文引导的聚合模块

3、门控传播策略

## 二、具体体现

### 1、Local relation learning

采用了和RS-CNN相同的初始特征：

![image-20210322145752030](https://gitee.com/suyunzzz/img/raw/master/img/20210322145752.png)

![image-20210322145813666](https://gitee.com/suyunzzz/img/raw/master/img/20210322145813.png)

### 2、Multi-scale context-guided aggregation

一个self-attention机制的使用：

![image-20210322150636599](https://gitee.com/suyunzzz/img/raw/master/img/20210322150636.png)

### 3、Gated propagation

enc代表高分辨率的点云，dec代表低分辨率的点云，通过计算一个权重来融合两种分辨率的特征。

![image-20210322151213154](https://gitee.com/suyunzzz/img/raw/master/img/20210322151213.png)

4、Loss function

多尺度交叉熵损失

![image-20210322151703588](https://gitee.com/suyunzzz/img/raw/master/img/20210322151703.png)



## 三、实验

![image-20210322152434413](https://gitee.com/suyunzzz/img/raw/master/img/20210322152434.png)

![image-20210322152445866](https://gitee.com/suyunzzz/img/raw/master/img/20210322152445.png)

消融实验

![image-20210322162307120](https://gitee.com/suyunzzz/img/raw/master/img/20210322162307.png)

